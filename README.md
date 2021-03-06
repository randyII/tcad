# TCAD

## Twitch Chat Analyzer DANKHACKERMANS
![](https://cdn.betterttv.net/emote/5e37903f61ff6b51e652837c/3x)
<br />
This application creates templates for posts on reddit which represents some usless statistics(SUS) about a stream based on twitch chat log.

###### Help to pay bills for servers:
<a href="https://www.buymeacoffee.com/QUlswbe" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-green.png" alt="Buy Me A Coffee" style="height: 35px !important;width: 150px !important;" ></a>

## Architecture

### Viewer
***Service***: `viewer` listens to multiple twich channels which via env variable `CHANNELS` and writes all data in postgres database in database.   <br />
**Note**: present code is garbanzo, do not look in src for viewer , I'm working on it.    <br />
**Requirments**: 
- `DBHOST`: database host
- `DBUSER`: postgres username 
- `DB`: database in postgres
- `DBPASSWORD`: postgres database password
- `NAME`: twitch bot/user name
- `API_KEY`: twtich chat api key(not the same as TW_API_KEY - make a google requests - "chatbots twitch" - https://en.lmgtfy.com/?q=chatbots+twitch&pp=1)
- `CHANNELS`: twtich channels which you want to listen (I'm listening to #pokelawls, #nmplol, #forsen, #sodapoppin, #greekgodx)
- `TW_API_KEY`: twitch api key

### Database
***Service***: `database` store all logs data. Table looks like this 
```
 id | username  | msg  |         timestamp          
----+-----------+------+----------------------------
```

### Analyzer
***Service***: `analyzer` makes some useless analitics for parsed data(like top 10 ppl who spammed the most or 10 the most spammed words or searching for word fat and ugly and stupid and dumb(all words in a file:`searchablew` )) in database and returns a zip file. <br />
**Note**: currently code is also a shitshow,I'm also working on it.<br />
**Requirments**:<br />
- `DBHOST`: database host
- `DBUSER`: postgres username 
- `DB`: database in postgres
- `DBPASSWORD`: postgres database password

## Quick start:
### Cloud/AWS deployment 
App can be deployed on 2 AWS t2.micro instances like that:
```
aws cloudformation create-stack --template-body file://Deployment.yml --stack-name viewer --parameters \
    ParameterKey=ListnerUsername,ParameterValue=${NAME} \
    ParameterKey=TwitchApiKey,ParameterValue=${API_KEY} \
    ParameterKey=Channels,ParameterValue='sodapoppin:pokelawls:greekgodx:nmplol:forsen' \
    ParameterKey=Channel,ParameterValue=sodapoppin \
    ParameterKey=KeyName,ParameterValue=test-key \
    ParameterKey=TwApi,ParameterValue=${TWAPI} \
    ParameterKey=DBPASS,ParameterValue=${DBPASS}
```
### Local deployment:
Or it can be tested locally. <br />
This should work if you define all env variables that required. (`${DBPASS}`,`${TWAPI}`, `${CHANNEL}`, `${API_KEY}`, `${NAME}`) <br />
You can see it in field environment in file - docker-compose.yml  <br />
```
docker-compose up 
```

