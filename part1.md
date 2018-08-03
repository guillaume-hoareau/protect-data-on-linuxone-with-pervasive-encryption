aes-128-cbc     188805.18k   681182.20k  1933875.38k  3380562.17k  5023402.43k
```
The last line is quite interesting, it shows the encrypion bandwidth of one IFL - encrypting blocks of 8192 bytes of data over and over, as fast as possible. In this case, the observed throughput is roughly 5 GB/s (5023402.43/(1024\*1024)).

Let's test now the decryption capabilities. Please issue the following command:
```
root@crypt06:~# openssl speed -evp aes-128-cbc -decrypt
Doing aes-128-cbc for 3s on 16 size blocks: 33183167 aes-128-cbc's in 2.86s
Doing aes-128-cbc for 3s on 64 size blocks: 32184259 aes-128-cbc's in 2.87s
Doing aes-128-cbc for 3s on 256 size blocks: 27682462 aes-128-cbc's in 2.88s
Doing aes-128-cbc for 3s on 1024 size blocks: 16928435 aes-128-cbc's in 2.91s
Doing aes-128-cbc for 3s on 8192 size blocks: 5391831 aes-128-cbc's in 2.93s
OpenSSL 1.0.2g  1 Mar 2016
built on: reproducible build, date unspecified
options:bn(64,64) rc4(8x,char) des(idx,cisc,16,int) aes(partial) blowfish(idx) 
compiler: cc -I. -I.. -I../include  -fPIC -DOPENSSL_PIC -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DB_ENDIAN -g -O2 -fstack-protector-strong -Wformat -Werror=format-security -Wdate-time -D_FORTIFY_SOURCE=2 -Wl,-Bsymbolic-functions -Wl,-z,relro -Wa,--noexecstack -Wall -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_GF2m -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DAES_ASM -DAES_CTR_ASM -DAES_XTS_ASM -DGHASH_ASM
The 'numbers' are in 1000s of bytes per second processed.
type             16 bytes     64 bytes    256 bytes   1024 bytes   8192 bytes
aes-128-cbc     185640.10k   717697.76k  2460663.29k  5956947.57k 15075044.22k
```
Same computation, as you can see the observed throuthput is 15 GB/s. That is normal because the CBC mode of operation associated with the AES encryption algorithm is faster in decryption that in encryption.

Let's check now how much crypto we offload to the hardware. Please issue the following command:
```
root@crypt06:~# icastats
 function     |          # hardware      |       # software
--------------+--------------------------+-------------------------
              |       ENC    CRYPT   DEC |        ENC    CRYPT   DEC
--------------+--------------------------+-------------------------
        SHA-1 |             236          |                0
      SHA-224 |               0          |                0
      SHA-256 |               0          |                0
      SHA-384 |               0          |                0
      SHA-512 |               0          |                0
        GHASH |               0          |                0
        P_RNG |               0          |                0
 DRBG-SHA-512 |            1014          |                0
       RSA-ME |               0          |                0
      RSA-CRT |               0          |                0
      DES ECB |         0              0 |         0             0
      DES CBC |         0              0 |         0             0
      DES OFB |         0              0 |         0             0
      DES CFB |         0              0 |         0             0
      DES CTR |         0              0 |         0             0
     DES CMAC |         0              0 |         0             0
     3DES ECB |         0              0 |         0             0
     3DES CBC |         0              0 |         0             0
     3DES OFB |         0              0 |         0             0
     3DES CFB |         0              0 |         0             0
     3DES CTR |         0              0 |         0             0
    3DES CMAC |         0              0 |         0             0
      AES ECB |         0              0 |         0             0
      AES CBC |  96681976      115370154 |         0             0
      AES OFB |         0              0 |         0             0
      AES CFB |         0              0 |         0             0
      AES CTR |         0              0 |         0             0
     AES CMAC |         0              0 |         0             0
      AES XTS |         0              0 |         0             0
```
**Note:** We can clearly see here the crypto offload in encryption operations. 96681976 operations were offloaded to the CPACF.
**Note2:** We can clearly see here the crypto offload in decryption operations. 115370154 operations were offloaded to the CPACF.

#### 3.2. Testing Pervasive encryption with scp
The scp command allows you to copy files over ssh connections. This is pretty useful if you want to transport files between computers, for example to backup something. The scp command uses the ssh protocol and they are very much alike. However, there are some important differences.
The scp command can be used in three* ways: to copy from a (remote) server to your computer, to copy from your computer to a (remote) server, and to copy from a (remote) server to another (remote) server. In the third case, the data is transferred directly between the servers; your own computer will only tell the servers what to do. These options are very useful for a lot of things that require files to be transferred
Let's first clean the icastats monitoring. Please issue the following command:
```
root@crypt06:~# icastats -r
```
The previous command reset the icastats monitoring interface, and ease to interpret future result. 

To confirm the reset took place, please issue the following command:
```
root@crypt06:~# icastats
 function     |          # hardware      |       # software
--------------+--------------------------+-------------------------
              |       ENC    CRYPT   DEC |        ENC    CRYPT   DEC
--------------+--------------------------+-------------------------
        SHA-1 |               0          |                0
      SHA-224 |               0          |                0
      SHA-256 |               0          |                0
      SHA-384 |               0          |                0
      SHA-512 |               0          |                0
        GHASH |               0          |                0
        P_RNG |               0          |                0
 DRBG-SHA-512 |               0          |                0
       RSA-ME |               0          |                0
      RSA-CRT |               0          |                0
      DES ECB |         0              0 |         0             0
      DES CBC |         0              0 |         0             0
      DES OFB |         0              0 |         0             0
      DES CFB |         0              0 |         0             0
      DES CTR |         0              0 |         0             0
     DES CMAC |         0              0 |         0             0
     3DES ECB |         0              0 |         0             0
     3DES CBC |         0              0 |         0             0
     3DES OFB |         0              0 |         0             0
     3DES CFB |         0              0 |         0             0
     3DES CTR |         0              0 |         0             0
    3DES CMAC |         0              0 |         0             0
      AES ECB |         0              0 |         0             0
      AES CBC |         0              0 |         0             0
      AES OFB |         0              0 |         0             0
      AES CFB |         0              0 |         0             0
      AES CTR |         0              0 |         0             0
     AES CMAC |         0              0 |         0             0
      AES XTS |         0              0 |         0             0
```

Let's now create a 512MB file that we will use later for a secure file transfer. 

Please issue the following command and note that it will take less than a minute:
```
root@crypt06:~# dd if=/dev/urandom of=data.512M bs=64M count=8 iflag=fullblock
8+0 records in
8+0 records out
536870912 bytes (537 MB, 512 MiB) copied, 45.9584 s, 11.7 MB/s
```
You just created a 512MB file made of random value. The file is named data.512MB.

You can confirm it issuing the following command:
```
root@crypt06:~# ls -hs
total 512M
512M data.512M
```
Now, let's see the hardware crypto offload with the SCP network protocol.

Please issue the following command, yes confirm if prompted, and enter your session password if prompted (azerty11):
```
root@crypt06:~# scp data.512M localhost:/dev/null
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:IRiJuwD8Z8NF77bRUfNIfcIYEbocWMVT6RVcZh+FChs.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
root@localhost's password: <----- azerty11

data.512M                                                         100%  512MB 170.7MB/s   00:03
```
As you can see, the defaut cipher suite transfered 512MB in 3 seconds, so a bandwidth of 170MB/s. Not so bad.

Issue the icastats command again to validate the hardware crypto offload:
```
root@crypt06:~# icastats
 function     |          # hardware      |       # software
--------------+--------------------------+-------------------------
              |       ENC    CRYPT   DEC |        ENC    CRYPT   DEC
--------------+--------------------------+-------------------------
        SHA-1 |             388          |                0
      SHA-224 |               0          |                0
      SHA-256 |              70          |                0
      SHA-384 |               0          |                0
      SHA-512 |               0          |                0
        GHASH |               0          |                0
        P_RNG |               0          |                0
 DRBG-SHA-512 |             338          |                0
       RSA-ME |               0          |                0
      RSA-CRT |               0          |                0
      DES ECB |         0              0 |         0             0
      DES CBC |         0              0 |         0             0
      DES OFB |         0              0 |         0             0
      DES CFB |         0              0 |         0             0
      DES CTR |         0              0 |         0             0
     DES CMAC |         0              0 |         0             0
     3DES ECB |         0              0 |         0             0
     3DES CBC |         0              0 |         0             0
     3DES OFB |         0              0 |         0             0
     3DES CFB |         0              0 |         0             0
     3DES CTR |         0              0 |         0             0
    3DES CMAC |         0              0 |         0             0
      AES ECB |         0              0 |         0             0
      AES CBC |         0              0 |         0             0
      AES OFB |         0              0 |         0             0
      AES CFB |         0              0 |         0             0
      AES CTR |         0              0 |         0             0
     AES CMAC |         0              0 |         0             0
      AES XTS |         0              0 |         0             0
```
**Note:** Data integrity operation with sha1 and sha256 were correcly reported. AES is accelerated by default and is not reported because not passing through libica device driver.

**Note2:** By default, scp doesn't use the best ciphers of IBM Z and LinuxONE. However, it is possible to specify it manually with -c option.

Let's compare the defaut cipher performance with an optimized cipher. Please issue the following command:
```
root@crypt06:~# scp -c aes256-ctr data.512M localhost:/dev/null
root@localhost's password: 
data.512M                                                           100%  512MB 256.0MB/s   00:02
```
As you can see, with 256MB/s, we increased the throughput by 50%. So, beware default settings. Make sure to use hardware accelerated ciphers by IBM Z and LinuxONE.

You are now ready to go to part2.
