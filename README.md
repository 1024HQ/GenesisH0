# GenesisHash
นี่คือสคริปต์ python ที่ใช้ในการสร้าง genesis block. SHA256/scrypt/X11/X13/X15.

# Install
    apt-get install python3
    apt-get install python3-pip
    sudo pip3 install scrypt construct==2.5.2


module เสริมสำหรับทำอัลกลอลิทึ่มอื่น ๆ โดยให้ทำการติดตั้งเสริมเข้าไปจึงจะใช้งานได้<br>
x11 - https://github.com/1024HQ/x11-hash<br>
x13 - https://github.com/1024HQ/X13-PythonHash<br>
x15 - https://github.com/1024HQ/x15_hash<br>
    
### สร้าง Unixtimestamp
https://hi.in.th/unix/

Command ในการสร้าง genesis hash

    python genesis.py -z "1024 The Times 3/1/2021 Bitcoin 34k+ high price." -n 1024000000 -t 1610068203 -a scrypt -b 0x1e0ffff0

โดยจะเป็นการสร้าง Genesis hash ด้วยประโยค "1024 The Times 3/1/2021 Bitcoin 34k+ high price."
nonce = 1024000000
unixtime = 1610068203 // จะเป็นบันทึกเวลาลงไปในบล็อคนี้ คือบล็อคแรกของเรานั่นเอง
algorithm = scrypt // เข้ารหัสแบบ scrypt
BITS = 0x1e0ffff0 // เป็นตัวที่เกี่ยวข้องกับค่าความยากฐานของระบบ โดย mainnet และ testnet จะใช้ 0x1e0ffff0 ส่วน Regression test จะใช้ 0x207fffff กรณีของ scrypt

Output:

    algorithm: scrypt
    merkle hash: f8c5e867340192a1950e79df83fc4555eadb881cbcfd1c1189b91efdbd4ec9af
    pszTimestamp: 1024 The Times 3/1/2021 Bitcoin 34k+ high price.
    pubkey: 04678afdb0fe5548271967f1a67130b7105cd6a828e03909a67962e0ea1f61deb649f6bc3f4cef38c4f35504e51ec112de5c384df7ba0b8d578a4c702b6bf11d5f
    time: 1610068203
    bits: 0x1e0ffff0
    Searching for genesis hash..
    genesis hash found!
    nonce: 1365269
    genesis hash: 8b1cbfb0bf82e0d36559e5585461ae65b3b4428dde8f196f4214d32c0c40127e

หลังจากสร้างบล็อคแรกได้แล้ว เราจะได้ pubkey ให้เรานำมาเก็บไว้ใช้ในการสร้างบล็อคให้กับ testnet ดังนี้

    python genesis.py -z "1024 The Times 3/1/2021 Bitcoin 34k+ high price." -n 1024001024 -t 1610068203 -a scrypt -b 0x1e0ffff0 -p "04678afdb0fe5548271967f1a67130b7105cd6a828e03909a67962e0ea1f61deb649f6bc3f4cef38c4f35504e51ec112de5c384df7ba0b8d578a4c702b6bf11d5f"
    
ต่อด้วย Regression test ปิดท้าย

    python genesis.py -z "1024 The Times 3/1/2021 Bitcoin 34k+ high price." -n 0 -t 1610068203 -a scrypt -b 0x207fffff -p "04678afdb0fe5548271967f1a67130b7105cd6a828e03909a67962e0ea1f61deb649f6bc3f4cef38c4f35504e51ec112de5c384df7ba0b8d578a4c702b6bf11d5f"

ตัวอย่างการสร้าง genesis hash ของ Litecoin

    python genesis.py -a scrypt -z "NY Times 05/Oct/2011 Steve Jobs, Apple’s Visionary, Dies at 56" -p "040184710fa689ad5023690c80f3a49c8f13f8d45b8c857fbcbc8bc4a8e4d3eb4b10f4d4604fa08dce601aaf0f470216fe1b51850b4acf21b179c45070ac7b03a9" -t 1317972665 -n 2084524493
    
    

### Options
    Usage: genesis.py [options]
    
    Options:
      -h, --help show this help message and exit
      -t TIME, --time=TIME  the (unix) time when the genesisblock is created
      -z TIMESTAMP, --timestamp=TIMESTAMP
         the pszTimestamp found in the coinbase of the genesisblock
      -n NONCE, --nonce=NONCE
         the first value of the nonce that will be incremented
         when searching the genesis hash
      -a ALGORITHM, --algorithm=ALGORITHM
         the PoW algorithm: [SHA256|scrypt|X11|X13|X15]
      -p PUBKEY, --pubkey=PUBKEY
         the pubkey found in the output script
      -v VALUE, --value=VALUE
         the value in coins for the output, full value (exp. in bitcoin 5000000000 - To get other coins value: Block Value * 100000000)
      -b BITS, --bits=BITS
         the target in compact representation, associated to a difficulty of 1

