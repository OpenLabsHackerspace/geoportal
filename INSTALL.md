# GDAL library

<https://trac.osgeo.org/gdal/wiki/BuildingOnUnix>

## Install required librairies

    sudo apt-get install build-essential libpq-dev

## Download GDAL

    wget http://download.osgeo.org/gdal/CURRENT/gdal-2.2.3.tar.gz
    tar -zxvf gdal-2.2.3.tar.gz
    cd gdal-2.2.3
    
## Build GDAL

    ./configure --with-pg
    make
    
## Install GDAL

    sudo make install
    
## Test

    gdalinfo --version
    
It should display :
    
    GDAL 2.2.3, released 2017/11/20

# MapServer

<http://mapserver.org/installation/unix.html>

## Install required librairies

    sudo apt-get install cmake \
      libpng++-dev libfreetype6-dev libjpeg-dev \
      libproj-dev libcurl4-gnutls-dev \
      libgeos-dev libxml2-dev libgif-dev libfcgi-dev libfribidi-dev libharfbuzz-dev \
      libcairo2-dev

## Download MapServer

    wget http://download.osgeo.org/mapserver/mapserver-7.0.7.tar.gz
    tar -zxvf mapserver-7.0.7.tar.gz
    cd mapserver-7.0.7
    
## Build MapServer

    mkdir build
    cd build
    cmake -DWITH_CLIENT_WMS=ON -DWITH_CLIENT_WFS=ON -DWITH_CURL=ON ..
    make

## Install MapServer

    sudo make install
    
## Test

    mapserv -v
    
It should display :

    MapServer version 7.0.7 OUTPUT=PNG OUTPUT=JPEG SUPPORTS=PROJ SUPPORTS=AGG SUPPORTS=FREETYPE SUPPORTS=CAIRO SUPPORTS=ICONV SUPPORTS=FRIBIDI SUPPORTS=WMS_SERVR SUPPORTS=WMS_CLIENT SUPPORTS=WFS_SERVER SUPPORTS=WFS_CLIENT SUPPORTS=WCS_SERVER SUPPORTS=FASTCGI SUPPORTS=GEOS INPUT=JPEG INPUT=POSTGIS INPUT=OGR INPUT=GDL INPUT=SHAPEFILE

# Apache

## Install Apache

    sudo apt-get install apache2
    
## Configure Apache

    sudo a2enmod cgid
    sudo service apache2 restart
    
    sudo ln -s /usr/local/bin/mapserv /usr/lib/cgi-bin/mapserv
    sudo ln -s ~/openlabs-geoportal/mapserver/fonts.txt /usr/lib/cgi-bin/fonts.txt
    sudo ln -s ~/openlabs-geoportal/mapserver/symbols.txt /usr/lib/cgi-bin/symbols.txt
    
    sudo cp ~/openlabs-geoportal/mapserver/mapserver.conf /etc/apache2/conf-available/mapserver.conf
    sudo a2enconf mapserver
    sudo service apache2 restart

# MapCache

<http://mapserver.org/mapcache/install.html>

## Install required librairies

    sudo apt-get install libaprutil1-dev libapr1-dev apache2-dev libsqlite3-dev libarchive-dev

## Download MapCache

    git clone git://github.com/mapserver/mapcache.git

## Build MapCache

    cd mapcache
    mkdir build
    cd build
    cmake ..
    make
    
## Install MapCache

    sudo make install

## Install MapCache Apache module

    sudo cp ~/openlabs-geoportal/mapcache/mapcache.conf /etc/apache2/mods-available/mapcache.conf
    sudo cp ~/openlabs-geoportal/mapcache/mapcache.load /etc/apache2/mods-available/mapcache.load
    sudo a2enmod mapcache
    sudo service apache2 restart
    
You'll maybe need to fix the path defined in `/etc/apache2/mods-available/mapcache.conf`
