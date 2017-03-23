# Vue Switches VMS
Original [drewjbartlett](https://github.com/drewjbartlett/vue-switches)

<img src="http://i1123.photobucket.com/albums/l554/DoctorOz_Oz/swi.png" />


```bash
    npm install vue-switches-vms --save
```

## Basic Usage

```javascript
import Switches from 'vue-switches-vms';

new Vue({

    components: {
        Switches
    },

    data () {
        return {
            enabled: false
        }
    }
};
```

```html

<switches v-model="enabled" :selected="enabled"></switches>

```

```html

<switches v-model="enabled" :callBack="nameFunction(enabled)" :selected="enabled"></switches>

```

## Props

`label` - A static label to always display whether on or off. <br />
`text-enabled` - The text that displays when enabled. <br />
`text-disabled` - The text that displays when disabled. <br />
`theme` - Which theme to use. <br />
`color` - Which color to use. <br />
`type-bold` - Bigger style. <br />
