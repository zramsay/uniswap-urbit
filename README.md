# Uniswap for Urbit

```
yarn install
```

To generate the "build" output folder: 
```
yarn build
```

Now inside of Urbit (a fake ship):
```
|merge %uniswap our %garden
|mount %uniswap
```

Copy the "build" folder from above into the root directory
of the uniswap desk.  I.e. the uniswap folder should have
`app/` `build/` `gen/` etc. inside of it.
```
|commit %uniswap
-garden!make-glob %uniswap /build
```
Now an output glob will exist inside `.urb/put/` in your fakeship.

This will be uploaded to IPFS or S3 and referenced inside the desk.docket-0 file.
now inside `%uniswap`:
- Delete desk.bill and desk.ship
- Update desk.docket-0, with the link and hash of the new glob inside `glob-http`, for example:
```
:~  title+'Uniswap'
    info+'Self-hosted uniswap frontend.'
    color+0xcd.75df
    image+'https://logowik.com/content/uploads/images/uniswap-uni7403.jpg'
    base+'uniswap'
    glob-http+['https://gateway.pinata.cloud/ipfs/QmfJPMitB1F8k5ekFB8YZcYTPXFTF9gCbBXHjmUSqVcUFZ' 0vsb6jg.ak54q.db64h.qaklb.dahuk]
    version+[0 0 1]
    website+'https://uniswap.org/'
    license+'MIT'
==
```
then:
```
|commit %uniswap
|install our %uniswap
```
and launch!
