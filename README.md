## Steps to reproduce

### Render slide(s)

Run this script from the root directory of this repository:

```sh
cd slidev
npm i
# define slides array in slides.js
echo "const data = [" >> ../public/slides.js
# render each slide separately
for SLIDE in $(echo *.md); do
    echo "ℹ️ Rendering slide ${SLIDE}"
    SLIDE_NAME=${SLIDE/.md/}
    # create temporary output dir for slide
    mkdir -p ../public/${SLIDE_NAME}
    # build slide
    npx slidev build --base /${SLIDE_NAME}/ --output ../public/${SLIDE_NAME} ${SLIDE}
    # add img assets to output dir
    mkdir ../public/${SLIDE_NAME}/img
    cp img/* ../public/${SLIDE_NAME}/img
    # add slide to array in slides.js
    echo \"${SLIDE_NAME}\", >> ../public/slides.js
    echo "✅ Done rendering slide ${SLIDE}"
done
# close slides array in slides.js
echo "];" >> ../public/slides.js
# add index page to outdir
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
