# form-picker

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

## Relevant Directories


### `components`

Component setup is based on [Atomic Design by Brad Frost](https://atomicdesign.bradfrost.com/). 

This project contains 2 Atoms 
- InputRadioGroup.vue
- InputDatePicker.vue

And 1 Molecule
- CheckAvailabilityForm.vue

### `pages`

The index page was the only necessary page as this is a single page application.

### `plugins`

The plugins directory contains a custom plugin for Pikaday's datepicker component.
