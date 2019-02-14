# sample-iot-thermostat
A sample configuration for Home Assistant and EMQX mqtt broker in clusterized docker environment.
In the configuration files, you need to modify some elements to fit your configuration environment.

configuration.yaml:
  base_url: YOUR_DOMAIN 

  mqtt:
    broker: YOUR_MQTT_BROKER_IP

  sensor1:
    state_topic: YOUR_BACKYARD_TEMPERATURE_SENSOR_TOPIC

  sensor2:
    state_topic: YOUR_HUMIDITY_SENSOR_TOPIC

  sensor3:
    state_topic: YOUR_INSIDE_TEMPERATURE_SENSOR_TOPIC

  switch 7:
    state_topic: YOUR_THERMOSTAT_SWITCH_STATE_TOPIC
    command_topic: YOUR_THERMOSTAT_SWITCH_COMMAND_TOPIC
  switch 8:
    state_topic: YOUR_THERMOSTAT_BACKLIGHT_SWITCH_TOPIC
    command_topic: YOUR_THERMOSTAT_BACKLIGHT_SWITCH_COMMAND_TOPIC
    
traefik.toml:
  [docker]
  domain = YOUR_DOMAIN

  [acme]
  email = YOUR_MAIL_ADDRESS
  storage = ACME_JSON_FILE_PATH

  [[acme.domains]]
    main = YOUR_DOMAIN
    
docker-compose.yml:
services:

  redis:
    volumes:
      - YOUR_PATH_TO_REDIS_CONFIG:/usr/local/etc/redis/redis.conf
  
  reverse-proxy:
    volumes:
      - YOUR_PATH_TO_TRAEFIK_CONFIG:/etc/traefik
      
  emq-master:
    volumes:
      - YOUR_PATH_TO_CERTS:/opt/emqx/etc/certs/
    environment:
      - "EMQX_AUTH__REDIS__PASSWORD=YOUR_REDIS_PASSWORD"
      - "EMQX_AUTH__REDIS__PASSWORD_HASH=plain"
      - "EMQX_DASHBOARD__DEFAULT_USER__LOGIN=YOUR_EMQX_DASHBOARD_USER"
      - "EMQX_DASHBOARD__DEFAULT_USER__PASSWORD=YOUR_EMQX_DASHBOARD_PASSWORD"
      
   home-assistant-swarm:
    volumes:
      - YOUR_HOME_ASSISTANT_CONFIG:/config
