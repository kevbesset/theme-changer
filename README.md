# Theme changer

A VueJs component to handle live theme changing with custom animation created during the [#1PAM challenge](https://github.com/kevbesset/one-package-a-month).

## Usage

First, surround your app with the `ThemeLayer` component. Then, it provides several informations in order to trigger the changes :

- `currrentTheme`: object corresponding to the current theme selected. By default `currentTheme.value` returns `default`
- `changeTheme(theme: string)`: function that triggers the theme changing adding the next theme as a string.

For example : 
```vue
<template>
  <button @click="toggleTheme">toggle dark / default</button>
  <button @click="changeTheme('blue')">blue theme</button>
</template>

<script>
export default {
  name: "ThemeChanger",
  inject: ["currentTheme", "changeTheme"],
  methods: {
    toggleTheme() {
      this.changeTheme(
        this.currentTheme.value === "default" ? "dark" : "default"
      );
    },
  },
};
</script>
```

## Handle multiple themes

In this component, there are some examples handling `default`, `dark` and `blue` theme. Inside `ThemeLayer` we have styles declaring CSS variables such as `--theme-text-color` and `--theme-background-color` depending on the current theme selected.

Of course, you can create as much theme as you want by adding the CSS modifier with the name of the theme and set the variables. Also, you can add more CSS variables if you want to handle more details in your theme.

## Customize the animation

For now, we have a clip-path polygon transition animation but it is easily customisable. The animation is a VueJs transition named `theme` available at the end of the `ThemeLayer` styles.