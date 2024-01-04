
1. **Installs the Extra Packages for Enterprise Linux (EPEL)**
   - Installs the Extra Packages for Enterprise Linux (EPEL) repository on an Amazon Linux system. EPEL provides additional software packages that are not included in the default Amazon Linux repositories.

```bash
sudo amazon-linux-extras install epel -y
```

2. **install required packgaes and dependencies**
   - Installs various development libraries and tools that are necessary for building and running Redis. These include the GNU Compiler Collection (GCC), Jemalloc (a memory allocator), OpenSSL development libraries, and Tcl (Tool Command Language) along with its development libraries.

```bash
sudo yum install gcc jemalloc-devel openssl-devel tcl tcl-devel -y
```   
   

3. **Download SOurce Code and unzip**
   - Downloads the Redis source code archive (`redis-stable.tar.gz`) from the official Redis website.
   
```bash
sudo wget http://download.redis.io/redis-stable.tar.gz
```
```bash
sudo tar xvzf redis-stable.tar.gz
```

```bash
cd redis-stable
```

4. **build redis**
   - Builds Redis from the source code. The `BUILD_TLS=yes` option indicates that Redis should be built with support for Transport Layer Security (TLS), which is used for secure communication.

```bash
sudo make BUILD_TLS=yes
```

5. **Grant Executable Permissions**
   - Grants execute (`+x`) permission to the Redis command-line interface (`redis-cli`) binary. This allows the binary to be executed as a program. Copies the Redis CLI binary (`redis-cli`) to the `/usr/bin/` directory. Placing it in this directory allows you to run `redis-cli` from any location in the terminal without specifying the full path.

```bash
chmod a+x src/redis-cli
```

```bash
cp redis-cli /usr/bin/
```


6. **Connect to Redis Cluster**
   - Invokes the Redis command-line interface, connecting to a Redis server with the following options:
     - `-h`: Specifies the hostname or IP address of the Redis server (`your-cluster-info.cache.amazonaws.com`).
     - `-tls`: Enables TLS for secure communication.
     - `-a`: Provides the authentication password (`YOURPASSWORD`).
     - `-p`: Specifies the port number (`6379`) on which the Redis server is running.
     - `-c`: Specifies this if your redis cluster is created with Cluster mode enabled. (`redis-cli -c -h your-cluster-info.cache.amazonaws.com -tls -a YOURPASSWORD -p 6379`).
	 
```bash
redis-cli -h your-cluster-info.cache.amazonaws.com -tls -a YOURPASSWORD -p 6379
```
