Invidious needs one PostgreSQL database with five tables.

`channel_videos` Stores truncated video info, used to create user feeds  
`channels` Stores UCID and author name  
`nonces` Keeps track of tokens issued to prevent CSRF  
`users` Stores user info, such as preferences, username, subscriptions  
`videos` Stores video cache, used to create "top" page  

The table `videos` grows a lot and needs the most storage. You can clean it up using following commands:
```bash
$ sudo -i -u postgres
$ psql invidious -c "DELETE FROM nonces * WHERE expire < current_timestamp"
$ psql invidious -c "TRUNCATE TABLE videos"
$ exit
```

For regular maintenance you should add a cronjob for these commands
```bash
@weekly psql invidious -c "DELETE FROM nonces * WHERE expire < current_timestamp"
@weekly psql invidious -c "TRUNCATE TABLE videos"
```