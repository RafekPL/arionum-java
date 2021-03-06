Arionum Miner port for Java
====================
by ProgrammerDan, Jan 2018
based on https://github.com/arionum/miner a work by AroDev

Originally a fun evening project to port the PHP miner to Java that has become a bit of a passion.

Confirmed working, has been used to find blocks.

Includes a few improvements over the reference implementation, mostly with threaded handling of update requests and submit requests.

That way, the miner threads can keep on mining, and aren't held up by the update / submit cycles. 

Enjoy!

#### How to run 

Get dependency:

* Java 8 JRE (OpenJRE 8 and Oracle Java 8 both work just fine)

* On Linux or MacOSX, download the latest ".jar" release file

    * If you've got it on your desktop, you should be able to doubleclick it.

    * If that doesn't work, open a shell, go to the download folder, and run `./jar -java arionum-miner-java.jar`

* On Windows, download the latest ".exe" release file

    * Double click the .exe to run

* Follow interactive prompt -- if a default is offered, press "enter" key to accept it. I recommend using defaults.

* Configuration is automatically saved and used again when you open the miner up next

* Default config is `config.cfg` in the same folder as the .jar or .exe -- open it in any text editor to modify

#### To compile

Get depencies

* Maven

* git

* Java 8 SDK (OpenJDK8 and Oracle Java 8 both work just fine) 

Clone this repository, change directory into ```arionum-miner```

Execute ```mvn clean package``` -- this will build the miner

##### Summary:

Pick a place to install. For linux:

`sudo apt-get install maven git openjdk-8-jdk`

`git clone git://github.com/ProgrammerDan/arionum-java`

`cd arionum-java/arionum-miner`

`mvn clean package`

`./run-pool.sh your-address-here`

#### To run

##### For Linux / Mac OS:

Execute `java -jar arionum-miner-java.jar` and follow the prompts on screen

##### For Windows:

Execute `arionum-miner-java.exe` and follow the prompts on screen

##### For All:

Take the defaults to run the pool, pointing at http://aropool.com , using your wallet address. If you take the defaults, the "stable" or "standard" miner will run, using cores / 4.


##### For solo mining in Linux:

Execute `java -jar arionum-miner-java.jar` and follow the prompts on screen, choose "solo" instead of pool.

Please note solo hasn't been tested, but it's 99.9% the same code so should be fine. Pool has been extensively tested. 


##### General advice:

Rule of thumb is no more then 1 miner per 4 cores/vcores.

----------------------
# Comparison to php-miner

Similarities:

* Can be used for solo or pool mining

* Checks for valid address (on pool) before activating

* Command Line tool

* Same hashing functions, same CPU bounds -- both bind to a C library version of argon2i hashing, so both are fast

* Can run multiple instances if desired -- I recommend you use screen, tmux, or byobu for better monitoring

Differences:

* One JVM can run multiple hashers and share the same "update" requests to the pool, reducing traffic to the pool or node

* The hashers never stop -- the reference PHP implementation pauses the hasher during update requests and nonce submissions. The java hashers never stop.

* (Very slight) optimizations -- the Java miner has easy swap support for alternate "core" types, with "basic", "debug" and "experimental" for now; basic is equivalent to php-reference, debug is chatty giving runtime details, and experimental has some improvements to improve the amount of time the hasher spends computing argon2i hashes instead of other things.

* More information on active hashing activity -- screen updates every 15s with details on active hashers, how many hashes they have checked, and how close to finding shares or blocks

* Visual notice when a new block has started, with the "difficulty" for the new block, and your personal best DL for the prior block. Note that difficulty is inverse -- smaller is harder, larger is easier.

---------------------
### TODO

* ~~Add address checking as here: https://github.com/arionum/miner/commit/e14b696362fb79d60c4ff8bc651185740b8021d9~~ Done in commit dd5388c

* Colored screen output to better see labels / stats and keep track of hashers

* Auto-adaptive hashing -- automatically add more hasher instances until peak rate is achieved

* Variable intensity -- allow hashing to take a back seat to other computer activity

---------------------
### Known caveates

* For systems with many, many cores (24, 32) it may be necessary to run multiple instances of this miner. Try using multiple miners with a max of 8 hashers each; this has worked well in initial testing.

* Hash Rate is "best effort", and might be imprecise. Pool Hash Rate is based on self-reporting, so minor inaccuracies are not an issue. Difficulty is based on median time between block discovery.

---------------------
### Testing

If you encounter any problems, open an issue here or email me at programmerdan@gmail.com -- I'm also ProgrammerDan#7586 in the Arionum discord -- feel free to PM me. 

---------------------
### Advanced Use

See DOCS.md for advanced use, compilation, and other instructions.
