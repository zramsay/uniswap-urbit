# Uniswap for Urbit

The process is:
- build static files for a UI --> ensure they conform (no caps, provide mark files for all file extensions)


## Build static files
```
git clone https://github.com/Uniswap/interface.git
```
Create a `./build` directory by following the CI: https://github.com/Uniswap/interface/blob/main/.github/workflows/4-deploy-to-prod.yml#L25-L26 

TODO: see also below:

## Install Urbit

For your OS :
- https://docs.urbit.org/manual/getting-started/self-hosted/cli#2-install-urbit
- do not go to step 3

## Boot a fake ship

This will create a directory at `./zod`:

```
./urbit -F zod
```
wait like 5-10 minutes, eventually you'll be in the dojo:

```
~zod:dojo>
```

if you drop ssh or close your laptop...to get back in the dojo, run:

```
./zod/.run
```

To view your Urbit:
- go to: http://localhost:8080 or http://localhost:80 or sometimes no port is required (e.g., on digital ocean)
- in the dojo, type `+code` for the login password

## Setup the desk

In the dojo:
```
|merge %uniswap our %landscape
|mount %uniswap
```

Copy the static `build` folder from above into the root directory of the uniswap desk.  
```
cp -r ~/interface/build ~/zod/uniswap
```

File names cannot have uppercase characters, therefore, all the files in `~/zod/uniswap/build/fonts/` (and elsewhere, if applicable) must
be changed to lowercase, and then `~/zod/uniswap/build/index.html` updated to reflect those changes. As well, all filepaths in that `index.html` need
to be prefixed with `/apps/uniswap`. Building build the static files against a root of /apps/uniswap would solve this and probably the other bug too.

Fixup more files
1. in `~/zod/uniswap/mar/map.hoon` put the contents of [this file](https://github.com/urbit/urbit/blob/develop/pkg/arvo/mar/map.hoon)
2. `cp ~/zod/uniswap/mar/woff2.hoon ~/zod/uniswap/mar/woff.hoon` and edit the only line that has `woff2` to `woff`
3. delete these files:

```
rm ~/zod/uniswap/desk.bill
rm ~/zod/uniswap/desk.ship
```

Next, back in the dojo:
```
|commit %uniswap
```

Next, create an http glob:

```
-landscape!make-glob %uniswap /build
```

which is created inside `~/zod/.urb/put/`.

Upload that to IPFS/S3/DigitalOcean and reference it inside the `~/zod/uniswap/desk.docket-0` file, alongside its hash, for example:

```
:~  title+'Uniswap'
    info+'Self-hosted uniswap frontend.'
    color+0xcd.75df
    image+'https://logowik.com/content/uploads/images/uniswap-uni7403.jpg'
    base+'uniswap'
    glob-http+['https://urbit-uniswap-laconic.nyc3.digitaloceanspaces.com/glob-0v3.5in3n.an2ft.c89g2.b6nu6.qek41.glob' 0v3.5in3n.an2ft.c89g2.b6nu6.qek41]
    version+[0 0 1]
    website+'https://uniswap.org/'
    license+'MIT'
==
```

back in the dojo:

```
|commit %uniswap
|install our %uniswap
```

Finally, head back to the browser and the tile should be there, click on it.
