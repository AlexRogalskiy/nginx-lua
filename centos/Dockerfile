FROM centos:7

WORKDIR /app

RUN yum -y install tar make gcc-c++ openssl-devel pcre-devel zlib-devel git lua-devel

# LuaJIT
RUN curl -sL https://github.com/openresty/luajit2/archive/refs/tags/v2.1-20220310.tar.gz | tar xz
RUN cd luajit* && make PREFIX=/usr/local install

# ngx_devel_kit
RUN curl -sL https://github.com/vision5/ngx_devel_kit/archive/refs/tags/v0.3.1.tar.gz | tar xz && mv ngx_devel_kit-0.3.1 /usr/local/src/ngx_devel_kit

# lua-nginx-module
RUN curl -sL https://github.com/openresty/lua-nginx-module/archive/refs/tags/v0.10.21rc2.tar.gz | tar xz && mv lua-nginx-module-0.10.21rc2 /usr/local/src/lua-nginx-module

# nginx
RUN curl -sL http://nginx.org/download/nginx-1.20.1.tar.gz | tar xz
RUN cd nginx-1.20.1 && LUAJIT_LIB=/usr/local/lib LUAJIT_INC=/usr/local/include/luajit-2.1 ./configure --prefix=/usr/local/nginx \
    --with-ld-opt="-Wl,-rpath,/usr/local/lib" \
    --add-module=/usr/local/src/ngx_devel_kit \
    --add-module=/usr/local/src/lua-nginx-module && make && make install

# lua-resty-core
RUN git clone https://github.com/openresty/lua-resty-core.git
RUN cd lua-resty-core && make install PREFIX=/usr/local

# lua-resty-lrucache
RUN git clone https://github.com/openresty/lua-resty-lrucache.git
RUN cd lua-resty-lrucache && make install PREFIX=/usr/local

# docker build -f Dockerfile -t nginx-centos .
# docker run -v $(pwd)/nginx.conf:/usr/local/nginx/conf/nginx.conf -p 8085:8085 -it --rm nginx-centos /usr/local/nginx/sbin/nginx -g 'daemon off;'
