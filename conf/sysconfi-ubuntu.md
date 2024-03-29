# Install Dependencies in Ubuntu 20.04


## Install Python, Pip and Virtualenv

``` > sudo apt update
sudo apt install software-properties-common
sudo apt install python3 python3-pip - y
sudo apt install python3-dev python3-virtualenv python3-venv virtualenvwrapper -y
sudo -H pip3 install --upgrade pip

echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3" | sudo tee --append ~/.bashrc
echo "export WORKON_HOME=~/.virtualenvs" | sudo tee --append ~/.bashrc
echo "export VIRTUALENVWRAPPER_VIRTUALENV=/usr/bin/virtualenv" | sudo tee --append ~/.bashrc
echo "source /usr/share/virtualenvwrapper/virtualenvwrapper.sh" | sudo tee --append ~/.bashrc

source ~/.bashrc 
```


## Install GDAL and GRASS GIS

```be sure to have an updated system
sudo apt-get update && sudo apt-get upgrade -y

install PROJ:
sudo apt-get install libproj-dev proj-data proj-bin unzip -y

install GEOS:
sudo apt-get install libgeos-dev -y

install GDAL_
sudo apt-get install libgdal-dev python3-gdal gdal-bin -y

install PDAL (optional):
sudo apt-get install libpdal-dev pdal -y

recommended to give Python3 precedence over Python2 (which is end-of-life since 2019)
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 1

install GDAL in a Python environment:
pip install gdal==3.3.0

Install dependencies:

sudo apt-get install build-essential flex make bison gcc libgcc1 g++ ccache python3 python3-dev python3-opengl python3-wxgtk4.0 python3-dateutil libgsl-dev python3-numpy wx3.0-headers wx-common libwxgtk3.0-gtk3-dev libwxbase3.0-dev libncurses5-dev libbz2-dev zlib1g-dev gettext libtiff5-dev libpnglite-dev libcairo2 libcairo2-dev sqlite3 libsqlite3-dev  libpq-dev libreadline6-dev libfreetype6-dev libfftw3-3 libfftw3-dev libboost-thread-dev libboost-program-options-dev  libpdal-dev subversion libzstd-dev checkinstall libglu1-mesa-dev libxmu-dev ghostscript wget -y


Download GRASS GIS:


cd ~/ && wget https://github.com/OSGeo/grass/archive/refs/tags/7.8.7.tar.gz
cd ~/ && tar -xzvf 7.8.7.tar.gz
cd grass-7.8.7


"configure" source code for local machine (checks for CPU type etc):
MYCFLAGS='-O2 -fPIC -fno-common -fexceptions -std=gnu99 -fstack-protector -m64'
#MYCXXFLAGS=''
MYLDFLAGS='-Wl,--no-undefined -Wl,-z,now'

LDFLAGS="$MYLDFLAGS" CFLAGS="$MYCFLAGS" CXXFLAGS="$MYCXXFLAGS" ./configure \
  --with-cxx \
  --enable-largefile \
  --with-proj --with-proj-share=/usr/share/proj \
  --with-gdal=/usr/bin/gdal-config \
  --with-python \
  --with-geos \
  --with-sqlite \
  --with-nls \
  --with-zstd \
  --with-pdal \
  --with-cairo --with-cairo-ldflags=-lfontconfig \
  --with-freetype=yes --with-freetype-includes="/usr/include/freetype2/" \
  --with-wxwidgets \
  --with-fftw \
  --with-motif \
  --with-opengl-libs=/usr/include/GL \
  --with-postgres=yes --with-postgres-includes="/usr/include/postgresql" \
  --without-netcdf \
  --without-mysql \
  --without-odbc \
  --without-openmp \
  --without-ffmpeg

note: the more CPUs you have, the higher the -j number may be set to
here: build using 4 CPU cores
make -j4

sudo make install

sudo apt install grass
```
## Set GDALDATA environment variable:

``` echo "export GDAL_DATA=/usr/share/gdal" | sudo tee --append ~/.bashrc
echo "export PROJ_LIB=/usr/share/proj" | sudo tee --append ~/.bashrc
source ~/.bashrc
```
## Install PostgreSQL and PostGIS
```
Create the file repository configuration:
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

Import the repository signing key:
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

Update the package lists:
sudo apt-get update
```
## Install the latest version of PostgreSQL.
```
If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
sudo apt install postgresql-client-12 postgresql-common postgresql-12 postgresql-12-postgis-3 netcat postgresql-12-ogr-fdw postgresql-12-postgis-3-scripts postgresql-plpython3-12 postgresql-12-pgrouting postgresql-server-dev-12 postgresql-12-cron -y
```
## PostGIS basic configuration:
```
sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'admin';"
sudo -u postgres psql -c "CREATE USER inescc WITH SUPERUSER PASSWORD 'admin';"
sudo -u postgres psql -c "CREATE USER replicator WITH REPLICATION PASSWORD 'admin';"
sudo -u postgres psql -c "CREATE EXTENSION postgis;"
sudo -u postgres psql -c "CREATE EXTENSION postgis_topology;"
sudo -u postgres createdb postgis_template
sudo -u postgres psql -d postgis_template -c "UPDATE pg_database SET datistemplate=true WHERE datname='postgis_template'"
sudo -u postgres psql -d postgis_template -c "CREATE EXTENSION hstore;"
sudo -u postgres psql -d postgis_template -c "CREATE EXTENSION tablefunc;"
sudo -u postgres psql -d postgis_template -c "CREATE EXTENSION postgis;"
sudo -u postgres psql -d postgis_template -c "CREATE EXTENSION postgis_raster;"
sudo -u postgres psql -d postgis_template -c "CREATE EXTENSION postgis_topology;"
sudo -u postgres psql -d postgis_template -c "CREATE EXTENSION pgrouting;"
```
## Install OSMIUM and OSMOSIS
```
sudo apt install osmium-tool osmosis -y
```
## Install SAGA GIS (optional):
```
sudo apt install libwxgtk3.0-gtk3-dev libtiff5-dev libexpat-dev wx-common unixodbc-dev

sudo apt install g++ cmake cmake-qt-gui make libtool git

Downloading SAGA sources
cd ~
git clone git://git.code.sf.net/p/saga-gis/code saga-gis-code

# Compile SAGA
cd saga-gis-code
mkdir build
cd build && cmake ../saga-gis -DCMAKE_BUILD_TYPE=RELEASE -DWITH_TRIANGLE=OFF -DWITH_SYSTEM_SVM=ON -DWITH_DEV_TOOLS=OFF
cmake --build . --config Release
autoreconf -fi

./configure
make
sudo make install
```
## Install R:
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9

sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'

sudo apt update
sudo apt install r-base
```
## Install NodeJS and NPM:
```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.39.1/install.sh | bash

source ~/.profile

nvm install 16.14.0
```
## Install Angular globaly:
```
> npm install -g @angular/cli@12.0.3
```
## Install VScode
```
sudo apt update

package dependence:
sudo apt install software-properties-common apt-transport-https wget -y

add GPG Key:
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -

add repository:
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"

install vscode:
sudo apt install code

verify installation:
code --version
```
## Filezilla SFTP 
```
# To find ip ubuntu VM:
ip a

# Open ssh
sudo apt install openssh-server
```
