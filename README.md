<!-- 10:30 5 minutes -->

<!--Hook: So in the beginning of the class, we introduced the idea of CSS classes, but the only way to apply them was directly in a file.  With DOM manipulation, we made that a little easier and more dynamic.  Now we're about to make it even easier with Angular's built-in features.-->

# Angular UI: ngClass, CSS animations and ngAnimate

### Objectives
- **Dynamically apply** and remove css classes using `ng-class`
- **Animate** elements with CSS transitions or keyframe animations
- **Do animations** _the Angular way_ (see lesson 2)

## CSS Classes in Angular - Intro

As you know, Angular is a great, dynamic front-end framework. One of the things you'll do _most_ often when building a frontend is apply CSS classes to style elements on the page. Having the ability to change those styles based on _data_  can enable you to make some really dynamic interface designs. You certainly have the skills to do this with tools you've already learned, but Angular has a special directive just for this purpose – **ngClass**.

Once we've looked at how Angular helps us apply classes, we'll mix in some non-Angular-specific strategies to add subtle animation to our style with CSS. Little things like CSS animation can really boost the credibility of your design – _if_ you use it well. As a first step, you have to know how to use it at all.

In lesson 2, we'll see how to do animations 'the Angular way'.

<!-- Start by showing finished code in solution_code folder, so devs know basic functionality.-->

## Starter Code

Let's check out the starter code in `starter-code/app` and see what we're working with. The starter code is a basic Angular app using stuff you've mostly already seen. _(Note: we are stepping back for simplicity. There is no need to install with `bower` or run a server to test this code. Everything here is on the front-end.)_

Take a few minutes and browse through the code that's there and investigate how it works and what it's doing.

<!--10:35 10 minutes -->

## ngClass - Codealong

`ngClass` is a pretty simple directive. There are a couple of possible ways to use it, but it needs to know the name of a class and whether the element should have that class or not - it uses a value of `true` or `false` to decide if it should apply the class.

As you can imagine, `ngClass` can be applied to any HTML tag. If it can handle normal classes, it can handle `ngClass`. But keep in mind, `ngClass` is designed for adding/removing classes, so if you don't expect the classes to change, just use the normal `class="something"` attribute.

Now let's give this a shot: Like in our finished example, we want to apply a class of `error` – we've already got a style for that that will make the borders red so it's obvious something's wrong.

We need a boolean expression that will tell us if there's an "error" or not. For simplicity's sake, let's say that our only validation is whether the text of the input is four or more characters. The number here is arbitrary.

We've got this:

```html
<input type="text" id="new-winner-name" ng-model="awards.newWinner.name" ... >
```

Adding `ngClass` is as easy as:

```html
<input type="text" id="new-winner-name" ng-model="awards.newWinner.name" ng-class="{'error': awards.newWinner.name.length <= 4}" ... >
```

Let's pull that out:
```js
ng-class="{'error': awards.newWinner.name.length <= 4}"
```

`ng-class` is just an attribute we add, a directive like we've seen before. In the quotes is a javascript object, where the key is the class name and the value is an expression that evaluates to `true` or `false`.

The value associated with each key could be a plain boolean like `true`, a simple expression like `awards.newWinner.name.length <= 4` above, or even a function call. Or, you could have a function that returns the entire object inside the ng-class string. Angular just needs to be able to evaluate the ng-class string to read like:

```js
{'error': true}
```

Since ng-class translates the string to a JavaScript object, it's _easy_ to have multiple classes that get turned on/off via multiple dynamic booleans.

```js
{'error': true, 'wombat': false, 'super': true, 'so-cool': true}
```

Would equate to:

```html
<input ... class="error super so-cool" ... >
```

<!-- 10:45 10 minutes -->

## Angular Form Validation - Independent Practice

Angular also has built-in [form validation](https://docs.angularjs.org/guide/forms) for many form fields.

- Add a field to the form for a "reason" for the award, and update the list of winners to display why they won.
- Require the user to input a reason for the new winner. Try submitting the form with a blank reason, and see what happens. 
- Give the reason field a minimum length of three and a maximum length of thirty. (Hint: look at the docs for `input[text]`.) Now what happens if you submit the form with the wrong length reason?  Disable the submit button if the form is invalid (Hint: look up `$invalid`.)

<!-- 10:55 5 minutes -->

## CSS Transitions - Intro

Now that we've got that down, let's talk about our other sweet trick – animation in CSS.

CSS supports two main kinds of animation: **transitions** and **keyframe animations**. Angular has a wrapper for many of the base CSS animations called `ngAnimate`. Right now we'll start with those base CSS animations. 

They both result in styles changing over time, and neither one gives you any extra styling – you can only animate using CSS properties that already exist.

**Transitions** are generally used as a reaction to a user's action - something that changes when you hover or focus or click.

**Animations** have the ability to be more complicated, can be defined more precisely, and are more flexible. They're also more code. 

It's worth noting that just like visual design, animation is an art. If you really like using animation, take some time to research the basic principles of animation for interfaces. You (and your users) will be glad you did.

> **Important note:** Not all transitions or animations work on css-generated pseudo-elements. :before and :after work in some browsers, but not all. Test or check with a reliable source to make sure something works in your target browsers instead of assuming it will.

<!-- 11:00 5 minutes -->

## CSS Transition Demo

Let's start with transitions - you'll see that those are pretty simple to get started with.

Let's say, for example, when we hover over our form, it rights itself and moves a little bit:

![animated form](https://cloud.githubusercontent.com/assets/25366/9594904/8ba3527c-5016-11e5-94a5-f8affb95ffba.gif)

We can inspect our form to see how it's rotated and positioned at the beginning:

```css
form {
  /* ... */

  -ms-transform: rotate(-7deg); /* IE 9 */
  -webkit-transform: rotate(-7deg); /* Chrome, Safari, Opera */
  transform: rotate(-7deg);

  position: relative;
  left:-1rem;
  top: 0rem;
}
```

Then add a `:hover` effect that modifies those rotations and positions, something like:

```css
form:hover {
  top: -0.618rem;
  left: -0.382rem;
  -ms-transform: rotate(-2deg);
  -webkit-transform: rotate(-2deg);
  transform: rotate(-2deg);
}
```

Now the fun part:

```css
form {
  /* ... */
  transition: all 300ms ease-in-out;
}
```

The `all` means that all CSS properties will be affected by this transition. The `300ms` says the transition should last that long, in milliseconds, and the `ease-in-out` is one of a few choices that we have for _easing_. Easing makes the animation look more natural. You can list out individual properties with different transition effects like this:

```
div {
  transition: background 0.2s ease,
              padding 0.8s linear;
}
```

Note also that you can put the time in seconds.

<!-- 11:10 5 minutes -->

## CSS Animation Demo

Now before we try it ourselves, let's see an animation. Animations are triggered, remember. By default, they are triggered when an object appears or when a class is applied to an object.

Before we can trigger a keyframe animation, we have to _define_ it with keyframes. Imagine a timeline where you're saying "At this spot, do this. At this next spot, change that. At this other spot, it should look like this."

We do that by creating a special `@keyframes` rule, and we name it whatever we like:

```css
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  80% {
    opacity: 0.5;
  }
  100% {
    opacity: 1;
  }
}
```

Of course, nearly any properties can be changed in each frame, and nearly any percentages can be included. You could have 100 keyframes if you want, or just two.

Once you have an animation defined, here's how you'd use it:

```css
ol li {
  animation-duration: 800ms;
  animation-name: fadeIn;
}
```

Just like a function, we reference the animation with the name we invented.

**Try refreshing the page** – see how it now fades in? **Try adding a name.** See how it fades?

[Here is a great CSS animation library](https://daneden.github.io/animate.css/)

Grab one of the animations and add it to your `style.css`

<!-- 11:15 10 minutes -->

## Share & Discuss
- What's the difference between CSS transitions and animations? Where might you use one or the other?
- How do you write an `ngClass` with multiple CSS classes?
- When do you use `ngClass` vs. normal classes?



## Licensing
All content is licensed under a CC­BY­NC­SA 4.0 license.
All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.
