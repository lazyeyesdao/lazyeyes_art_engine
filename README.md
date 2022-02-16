# Source Code from "How To Create An ENTIRE NFT Collection (10,000+)" 👁

![](https://github.com/lazyeyesdao/lazyeyes_art_engine/blob/main/Logo.png)

Documentation: https://lazy-eyes.gitbook.io/overview/university/minting-contracts-and-storage-nfts

To find out more please visit:

[📺 TikTok](https://www.tiktok.com/@lazyeyes.site)

[👄 Discord](https://discord.gg/kkDdqRzSgu)

[💬 Telegram](https://t.me/lazyeyesdao_group)

[🐦 Twitter](https://twitter.com/lazyeyesdao)

[ℹ️ Website](https://lazyeyes.site/)

# LazyEyes Art Engine 🔥

Create generative art by using the canvas api and node js. Before you use the generation engine, make sure you have node.js(v10.18.0) installed.

## Installation 🛠️

If you are cloning the project then run this first, otherwise you can download the source code on the release page and skip this step.

    git clone https://github.com/lazyeyesdao/lazyeyes_art_engine

Go to the root of your folder and run this command if you have yarn installed.

    yarn install

Alternatively you can run this command if you have node installed.

    npm install

## Usage ℹ️

Create your different layers as folders in the 'layers' directory, and add all the layer assets in these directories. You can name the assets anything as long as it has a rarity weight attached in the file name like so: `example blue#70.png`. You can optionally change the delimiter `#` to anything you would like to use in the variable `rarityDelimiter` in the `src/config.js` file.

Once you have all your layers, go into `src/config.js` and update the `layerConfigurations` objects `layersOrder` array to be your layer folders name in order of the back layer to the front layer.

_Example:_ If you were creating a portrait design, you might have a background, then a head, a mouth, eyes, eyewear, and then headwear, so your `layersOrder` would look something like this:

```js
const layerConfigurations = [
  {
    growEditionSizeTo: 5,
    layersOrder: [
      { name: "Background" },
      { name: "Legs" },
      { name: "Eye" },
      { name: "Color" },
      { name: "Pupils" },
    ],
  },
];
```

The `name` of each layer object represents the name of the folder (in `/layers/`) that the images reside in.

Optionally you can now add multiple different `layerConfigurations` to your collection. Each configuration can be unique and have different layer orders, use the same layers or introduce new ones. This gives the artist flexibility when it comes to fine tuning their collections to their needs.

_Example:_ If you were creating a eye, you might have a background, then a legs, and then eye and you want to create a new collection or just simple re-order the layers or even introduce new layers, then you're `layerConfigurations` and `layersOrder` would look something like this:

```js
const layerConfigurations = [
  {
    // Creates up to 5 artworks
    growEditionSizeTo: 5,
    layersOrder: [
      { name: "Background" },
      { name: "Legs" },
      { name: "Eye" },
    ],
  },
  {
    // Creates an additional 5 artworks
    growEditionSizeTo: 10,
    layersOrder: [
      { name: "Background" },
      { name: "Legs" },
      { name: "Eye" },
      { name: "Color" },
      { name: "Pupils" },
    ],
  },
];
```

Update your `format` size, ie the outputted image size, and the `growEditionSizeTo` on each `layerConfigurations` object, which is the amount of variation outputted.

You can mix up the `layerConfigurations` order on how the images are saved by setting the variable `shuffleLayerConfigurations` in the `config.js` file to true. It is false by default and will save all images in numerical order.

If you want to have logs to debug and see what is happening when you generate images you can set the variable `debugLogs` in the `config.js` file to true. It is false by default, so you will only see general logs.

If you want to play around with different blending modes, you can add a `blend: MODE.colorBurn` field to the layersOrder `options` object.

If you need a layers to have a different opacity then you can add the `opacity: 0.7` field to the layersOrder `options` object as well.

If you want to have a layer _ignored_ in the DNA uniqueness check, you can set `bypassDNA: true` in the `options` object. This has the effect of making sure the rest of the traits are unique while not considering the `Background` Layers as traits, for example. The layers _are_ included in the final image.

To use a different metadata attribute name you can add the `displayName: "Lazy Eye"` to the `options` object. All options are optional and can be addes on the same layer if you want to.

Here is an example on how you can play around with both filter fields:

```js
const layerConfigurations = [
  {
    growEditionSizeTo: 5,
    layersOrder: [
      { name: "Background" , {
        options: {
          bypassDNA: false;
        }
      }},
      { name: "Legs" },
      {
        name: "Eye",
        options: {
          blend: MODE.destinationIn,
          opacity: 0.2,
          displayName: "Lazy Eye",
        },
      },
      { name: "Color", options: { blend: MODE.overlay, opacity: 0.7 } },
      { name: "Pupils" },
    ],
  },
];
```

Here is a list of the different blending modes that you can optionally use.

```js
const MODE = {
  sourceOver: "source-over",
  sourceIn: "source-in",
  sourceOut: "source-out",
  sourceAtop: "source-out",
  destinationOver: "destination-over",
  destinationIn: "destination-in",
  destinationOut: "destination-out",
  destinationAtop: "destination-atop",
  lighter: "lighter",
  copy: "copy",
  xor: "xor",
  multiply: "multiply",
  screen: "screen",
  overlay: "overlay",
  darken: "darken",
  lighten: "lighten",
  colorDodge: "color-dodge",
  colorBurn: "color-burn",
  hardLight: "hard-light",
  softLight: "soft-light",
  difference: "difference",
  exclusion: "exclusion",
  hue: "hue",
  saturation: "saturation",
  color: "color",
  luminosity: "luminosity",
};
```

When you are ready, run the following command and your outputted art will be in the `build/images` directory and the json in the `build/json` directory:

```sh
npm run build
```

or

```sh
node index.js
```

The program will output all the images in the `build/images` directory along with the metadata files in the `build/json` directory. Each collection will have a `_metadata.json` file that consists of all the metadata in the collection inside the `build/json` directory. The `build/json` folder also will contain all the single json files that represent each image file. The single json file of a image will look something like this:

```json
{
  "name": "Your Collection #1",
  "description": "Remember to replace this description",
  "image": "ipfs://NewUriToReplace/1.png",
  "dna": "6960fa3275250028a216b63ab998074ef38a9d05",
  "edition": 1,
  "date": 1644979280522,
  "external_url": "https://lazyeyes.site",
  "attributes": [
    {
      "trait_type": "Background",
      "value": "Blue"
    },
    {
      "trait_type": "Legs",
      "value": "Green"
    },
    {
      "trait_type": "Eye",
      "value": "Eye"
    },
    {
      "trait_type": "Color",
      "value": "Green"
    },
    {
      "trait_type": "Pupils",
      "value": "Round"
    }
  ],
  "compiler": "LazyEyes"
}
```

You can also add extra metadata to each metadata file by adding your extra items, (key: value) pairs to the `extraMetadata` object variable in the `config.js` file.

```js
const extraMetadata = {
  creator: "Lazy BOSS",
};
```

If you don't need extra metadata, simply leave the object empty. It is empty by default.

```js
const extraMetadata = {};
```

That's it, you're done.

## Utils

### Updating baseUri for IPFS and description

You might possibly want to update the baseUri and description after you have ran your collection. To update the baseUri and description simply run:

```sh
npm run update_info
```

### Generate a preview image

Create a preview image collage of your collection, run:

```sh
npm run preview
```

### Generate pixelated images from collection

In order to convert images into pixelated images you would need a list of images that you want to convert. So run the generator first.

Then simply run this command:

```sh
npm run pixelate
```

All your images will be outputted in the `/build/pixel_images` directory.
If you want to change the ratio of the pixelation then you can update the ratio property on the `pixelFormat` object in the `src/config.js` file. The lower the number on the left, the more pixelated the image will be.

```js
const pixelFormat = {
  ratio: 5 / 128,
};
```

### Generate GIF images from collection

In order to export gifs based on the layers created, you just need to set the export on the `gif` object in the `src/config.js` file to `true`. You can also play around with the `repeat`, `quality` and the `delay` of the exported gif.

Setting the `repeat: -1` will produce a one time render and `repeat: 0` will loop forever.

```js
const gif = {
  export: true,
  repeat: 0,
  quality: 100,
  delay: 500,
};
```

### Printing rarity data (Experimental feature)

To see the percentages of each attribute across your collection, run:

```sh
npm run rarity
```

The output will look something like this:

```sh
Trait type: Top lid
Trait type: Background
{ trait: 'Blue', chance: '1', occurrence: '40% out of 100%' }
{ trait: 'Gray', chance: '1', occurrence: '60% out of 100%' }
{ trait: 'Pink', chance: '1', occurrence: '0% out of 100%' }

Trait type: Legs
{ trait: 'Gray', chance: '1', occurrence: '0% out of 100%' }
{ trait: 'Green', chance: '1', occurrence: '40% out of 100%' }
{ trait: 'Purpule', chance: '1', occurrence: '60% out of 100%' }

Trait type: Eye
{ trait: 'Eye', chance: '1', occurrence: '100% out of 100%' }

Trait type: Color
{ trait: 'Green', chance: '1', occurrence: '60% out of 100%' }
{ trait: 'Purpule', chance: '1', occurrence: '0% out of 100%' }
{ trait: 'Yellow', chance: '1', occurrence: '40% out of 100%' }

Trait type: Pupils
{ trait: 'Round', chance: '1', occurrence: '100% out of 100%' }
```

Hope you create some awesome artworks with this code 👁

Base code is from: https://github.com/HashLips
