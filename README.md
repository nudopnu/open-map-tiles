# open-map-tiles 🌍
A simple starting setup for hosting your own [OpenStreetMap Vector Tiles](https://openmaptiles.org/).

## Prerequisites
Install [docker](https://www.docker.com/products/docker-desktop/)

## Run server

1. Extract the `data/test_data.zip` to the `data` folder.
2. Start the server with `docker compose up`
3. Visit http://localhost:8000/

### Generate custom region on windows (wsl)

You will need some geo data in [pbf format](https://wiki.openstreetmap.org/wiki/PBF_Format). You can download this e.g. from https://download.geofabrik.de/.

1. Run wsl ubuntu with `wsl --install`
2. Install make with `sudo apt install make`
3. Run `git clone https://github.com/openmaptiles/openmaptiles`
4. Setup your `data` folder with `mkdir -p ./openmaptiles/data && cd openmaptiles && explorer.exe .`
5. Paste your downloaded `<your-map>.osm.pbf` inside that `data` folder
6. Edit the `.env` file by setting `MAX_ZOOM=12` (or `14` if your computer can handle that)
7. Run `sudo ./quickstart.sh <your-map>` (You may need to enable wsl integration with your wsl distro in the docker desktop settings under `Settings` > `Resources` > `WSL integration`)
8. Access the generated data under `data/tiles.mbtiles`

### Set which zooms to generate
modify the settings in the .env file, the defaults:

```bash
MIN_ZOOM=0
MAX_ZOOM=7
```

Hints:

Small increments! Never starts with the `MAX_ZOOM = 14`
The suggested `MAX_ZOOM = 14` - use only with small extracts

### Generate custom region extract
1. Grab a larger map (or even world map) from https://download.geofabrik.de/
2. Visit https://boundingbox.klokantech.com/ to select a region
3. Export it at the bottom by selecting `CSV` as output
4. In wsl run `make bash` to enter a shell in docker
5. Run the following with your coordinates:
```bash
  osmconvert ./data/planet.osm.pbf \
        -b=-14.53,28.89,43.21,59.8 \
        -o=./data/part.osm.pbf
  ```
