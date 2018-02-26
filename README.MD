# Introduction
Hass component PS4 is a custom media_player component for home assistant, that uses the tool [ps4-waker](https://github.com/dhleong/ps4-waker) to communicate with your PS4 to display now playing info, as well as remote start/stop functionality.

## Changes
Uses PSN store API for dynamic thumbnail retrieval, including games and apps.

Because it uses the ps4-waker tool witch is nodejs and not python it has not been committed to the main repository.

# Prerequisite
The module requires you to have [ps4-waker](https://github.com/dhleong/ps4-waker) running already

* Install nodejs
* Install ps4-waker
* Download the Playstation app so you can setup a remote device
* Start the pairing process ps4-waker -c .ps4-wake.credentials.json -d <PS4_IP_ADDR>
* Save credentials file in the config dir, default location is .ps4-wake.credentials.json in the config dir
* Test that ps4-waker -c .ps4-wake.credentials.json -d <PS4_IP_ADDR> search works

This docker images has node and ps4-waker installed https://hub.docker.com/r/hnicolaisen/home-assistant

# Custom component
To use the ps4 custom component you need to install it by doing this :
1. Copy https://github.com/hmn/home-assistant-config/tree/master/custom_components/media_player to your config dir under custom_components/media_player
2. Add platform: ps4 to your configuration.yaml

Example config:
```yaml
media_player:
  - platform: ps4
    host: <PS4_IP_ADDR>
```

## Component options
* **host**: ip of the ps4
* **port**: ps4-waker bind port
* **name**: device name (Default: Playstation 4)
* **cmd**: ps4 waker command (Default: ps4-waker)
* **filename**: credentials filename for ps4-waker (Default: .ps4-wake.credentials.json)
* **games_filename**: games json file, data file of the games started on your playstation (Default: ps4-games.json)
* **imagemap_json**: imagemap json file (Default: https://github.com/hmn/ps4-imagemap/raw/master/games.json)
* **local_store**: local www folder to use instead of imagemap (Default: None)

### games json file example
The json games file is automaticly written and updates every time you start a game, but if you need to edit it manually the format is :
```json
{
	"CUSA02491": "Mass Effect\u2122", 
	"CUSA01116": "YouTube", 
	"CUSA01262": "Tom Clancy's The Division\u2122", 
	"CUSA01703": "Plex"
}
```

## Examples
Multiple Playstation example with seperate games list and credentials for each ps4
```yaml
media_player:
  - platform: ps4
    name: 'Living room PS4'
    host: <PS4_IP_ADDR>
    filename: '.ps4-wake.livingroom.credentials.json'
    games_filename: 'ps4-games-livingroom.json'
  - platform: ps4
    name: 'Games room PS4'
    host: <OTHER_PS4_IP_ADDR>
    filename: '.ps4-wake.gamesroom.credentials.json'
    games_filename: 'ps4-games-gamesroom.json'
```

Alternative ps4-waker run command with custom config files
```yaml
media_player:
  - platform: ps4
    host: <PS4_IP_ADDR>
    cmd: 'sudo ps4-waker'
    filename: 'ps4-wake.credentials.json'
    games_filename: 'ps4-games.json'
```

Load images from local www dir. Naming convention for images is www/{local_store folder}/{titleid}.jpg
```yaml
media_player:
  - platform: ps4
    host: <PS4_IP_ADDR>
    local_store: games
```

# Todo
Look into writing a python module to replace ps4-waker
