## Modifications of renderd and render_list for my particular usage scenario.

 * renderd, error logs
 * render_list stats to /tmp/render_list.stats
 * jemalloc appears to eat less memory in my usage scenario (~8GB vs ~40GB)


## Install

    sudo apt --yes install --reinstall ca-certificates

    # Install build dependencies
    sudo apt --no-install-recommends --yes install \
      apache2 \
      apache2-dev \
      cmake \
      curl \
      g++ \
      gcc \
      git \
      libcairo2-dev \
      libcurl4-openssl-dev \
      libglib2.0-dev \
      libiniparser-dev \
      libmapnik-dev

    # Download, Build & Install `mod_tile`
    export CMAKE_BUILD_PARALLEL_LEVEL=$(nproc)
    cd /tmp
    git clone --depth 1 https://github.com/normundsniemanis/mod_tile.git 
    cd mod_tile
    mkdir build
    cd build
    cmake -B . -S .. \
      -DCMAKE_BUILD_TYPE:STRING=Release \
      -DCMAKE_INSTALL_LOCALSTATEDIR:PATH=/var \
      -DCMAKE_INSTALL_PREFIX:PATH=/usr \
      -DCMAKE_INSTALL_RUNSTATEDIR:PATH=/run \
      -DCMAKE_INSTALL_SYSCONFDIR:PATH=/etc \
      -DMALLOC_LIB=jemalloc
    cmake --build .
    sudo cmake --install . --strip

Installed files to copy to Docker

    /var/cache/renderd/tiles
    /run/renderd
    /etc/apache2/mods-available/tile.load
    /etc/renderd.conf
    /usr/lib/apache2/modules/mod_tile.so
    /usr/bin/render_expired
    /usr/bin/render_list
    /usr/bin/render_old
    /usr/bin/render_speedtest
    /usr/bin/renderd
    /share/man/man1/render_expired.1
    /share/man/man1/render_list.1
    /share/man/man1/render_old.1
    /share/man/man1/render_speedtest.1
    /share/man/man1/renderd.1
    /share/man/man5/renderd.conf.5
