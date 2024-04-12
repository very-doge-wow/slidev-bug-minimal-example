## Steps to reproduce

### Render slide(s)

Run this script from the root directory of this repository:

```sh
cd slidev
npm i
mkdir render
# define slides array in slides.js
echo "const data = [" >> ./render/slides.js
# render each slide separately
for SLIDE in $(echo *.md); do
    echo "ℹ️ Rendering slide ${SLIDE}"
    SLIDE_NAME=${SLIDE/.md/}
    # create temporary output dir for slide
    mkdir -p render/${SLIDE_NAME}
    # build slide
    npx slidev build --base /${SLIDE_NAME}/ ${SLIDE}
    # move dist to temporary output dir
    mv dist/* render/${SLIDE_NAME}
    # add img assets to output dir
    mkdir render/${SLIDE_NAME}/img
    cp img/* render/${SLIDE_NAME}/img
    # delete build dir
    rm -rf dist
    # add slide to array in slides.js
    echo \"${SLIDE_NAME}\", >> ./render/slides.js
    echo "✅ Done rendering slide ${SLIDE}"
done
# close slides array in slides.js
echo "];" >> ./render/slides.js
# move temporary output dir to final outdir
mv ./render ../public
# add index page to final outdir
mv ../index.html ../public
```

### Host in nginx Docker container

```sh
docker run -u 0 -it -p 8080:8080 -v $(pwd)/public:/usr/share/nginx/html -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf nginx
```

### Open hosted site

Open the site in your browser:

```
http://localhost:8080
```

And then click on the first slide `Example-Slide`.

Check the errors on the console log.
