> docker pull kietara/fluentd-plugin:1.0.0 

> docker pull kietara/eventrepo-backend:1.0.0

> docker pull kietara/eventrepo_frontend:1.0.0

#STEP 1 MONGO LOG FLUENTD
> sudo docker run -d --name fluend-log-mongo -p 27017:27017 mongo'

#STEP2 RUN Fluentd  in path  "/config.conf" is existing
> sudo docker run --name fluentd --rm -it -v $(pwd)/config.conf:/fluentd/etc/config.conf -e FLUENTD_CONF=config.conf -p 24224:24224 kietara/fluentd-plugin:1.0.0

#STEP3 RUN APP EventRepoBackend in root dir proejct (project at git.matador.ais.co.th)
> sudo docker run -it --rm --name eventrepo-backend -p 8291:8291 -v $(pwd)/config:/config -v $(pwd)/log:/log -v $(pwd)/libjava:/libjava kietara/eventrepo-backend:1.0.0

!ps . you can check config file in config/config.yml is match with env ?

#STEP4 RUN Frontend
> sudo docker run --rm -it --name eventrepo-frontend -e VUE_APP_API_EVENT=http://{IP-EVENTREPO-BACKEND}:8291 -p 8080:8080 kietara/eventrepo_frontend:1.0.0

#STEP 5 TRY AND SEE AT FLUNTD , MONGODB , LOGFILE

OUTPUT I GET : {public IP Eventrepo-Frontend}:8080<br>
OUTPUT II Log at eventrepo-backend in /log/*.log<br>
OUTPUT III MongoDB conected with fluend<br>
