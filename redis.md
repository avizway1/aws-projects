
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

3. **Download Source Code and unzip**
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
chmod a+x /src/redis-cli
```

```bash
cp /src/redis-cli /usr/bin/
```


6. **Connect to Redis Cluster**
   - Invokes the Redis command-line interface, connecting to a Redis server with the following options:
     - `-h`: Specifies the hostname or IP address of the Redis server (`your-cluster-info.cache.amazonaws.com`).
     - `-tls`: Enables TLS for secure communication.
     - `-a`: Provides the authentication password (`YOURPASSWORD`).
     - `-p`: Specifies the port number (`6379`) on which the Redis server is running.
	 
```bash
redis-cli -h your-cluster-info.cache.amazonaws.com -tls -a YOURPASSWORD -p 6379
```

If you want to use IAM authentication to connect to the Redis cluster, Follow below steps.


7. **Install Java, Git and naven**

- Install Git, Java and Mavaen to Clone and build AWS sample application


```bash
sudo amazon-linux-extras install java-openjdk11 -y
```

```bash
sudo yum install java-1.8.0-openjdk-devel -y
```

```bash
sudo update-alternatives --config java
```

8. Setup JAVA_HOME

```bash
echo $JAVA_HOME
```

```bash
export  JAVA_HOME='/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.392.b08-2.amzn2.0.1.x86_64/jre'
```

```bash
PATH=$JAVA_HOME/bin:$PATH
```

```bash
export PATH
```

```bash
source ~/.bashrc
```

9. Install Git to clone the Sample application

```bash
yum install git -y
```

**Maven Installation**

```bash
sudo wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
```

```bash
sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
```

```bash
sudo yum install -y apache-maven
```

```bash
mvn --version
```


```bash
git clone https://github.com/aws-samples/elasticache-iam-auth-demo-app.git
```

```bash
cd elasticache-iam-auth-demo-app
```

```bash
mvn dependency:resolve-plugins
```

10. **Prepare the build**

```bash
mvn clean install
```

11. **Make sure you create an IAM role and attach the role to your ec2 instance.**

Role should have policy to connect to cluster. Refer the sample IAM policy attached to the role. 
*Note: I have added wildcard for most of the Resources, You can limit to the users and replication groups.*

```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "elasticache:Connect",
            "Resource": [
                "arn:aws:elasticache:*:123456789123:user:*",
                "arn:aws:elasticache:*:123456789123:serverlesscache:*",
                "arn:aws:elasticache:*:123456789123:replicationgroup:*"
            ]
        }
    ]
}
```

12. **get your cluster info.**

**Now Create Users and Groups*, then proceed to connect to the cluster**

***Replace with your created cluster name, username***

**Get the required token to connect**

```bash
java -cp target/ElastiCacheIAMAuthDemoApp-1.0-SNAPSHOT.jar com.amazon.elasticache.IAMAuthTokenGeneratorApp --region ***ap-south-1*** --replication-group-id ***myredis*** --user-id ***avinash***
```

13. **Test connection using Demo-Application**

```bash
java -jar target/ElastiCacheIAMAuthDemoApp-1.0-SNAPSHOT.jar --redis-host ***master.myredis.77otcp.aps1.cache.amazonaws.com*** --region ***ap-south-1*** --replication-group-id ***myredis*** --user-id ***avinash*** --tls
```
