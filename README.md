JumpingBeans
============


The **JumpingBeans** make your test jump to the eye. Literally!

## What are the JumpingBeans
Have you ever used Hangouts? If not, do it and then come back here. Go. Go, I said!

Good. With that under our belt, we can be confident you've seen at least once those fancy,
nice jumping suspension dots that Hangouts uses to indicate that someone is typing, or some
other kind of ongoing activity (e.g., connecting to a video hangout).

Well, since there's no official naming for them, and since they remind me of the jumping
Mexican beans, the name for a library that emulates their behaviour has come to be exactly
that: **JumpingBeans**.


## What you can do
The library supports two main operation modes: **appending three jumping dots**,
Hangouts-style, or making any arbitrary subsection of a CharSequence jump, either as a
wave or as a single block.

### Append jumping dots
This method takes the trailing `...` (or appends them, if the given TextView's text
doesn't end in three dots), and makes them jump like it was -the 70s- Hangouts.

The defaults emulate the Hangouts L&F as closely as possible, but you can easily change
the animation properties to suit your needs.

### Make text jumping
This method takes the specified subsection a the TextView text and animates it as to
make it jump.

## Usage
Just create a `JumpingBeans` by using its `Builder` and call the method you want:

```java
// Append jumping dots
final TextView textView1 = (TextView) findViewById(R.id.jumping_text_1);
jumpingBeans1 = new JumpingBeans.Builder()
        .appendJumpingDots(textView1)
        .build();
        
// Make the first word's letters jump
final TextView textView2 = (TextView) findViewById(R.id.jumping_text_2);
jumpingBeans2 = new JumpingBeans.Builder()
        .makeTextJump(textView2, 0, textView2.getText().toString().indexOf(' '))
        .build();
```

### Customising the jumpin' beans
Just act on the `Builder`. Don't want the dots to jump in a wave? Call 
`setIsWave(false)`. Don't like the default loop duration? `setLoopDuration(int)`
is here to help. Fancy different per-char delays in waves? Well, ya know that
`setWavePerCharDelay(int)` is the one you want. Maybe you wanted to have a
shorter pause between jumping cycles? BAM, `setAnimatedDutyCycle(float)` and
you're all set.

### Being a responsible citizen
Since Spans were not really designed to be animated, there's some trickery
going on behind the scenes to make this happen. You needn't be concerned with it,
but **make sure you call the `stopJumping()` method** on your `JumpingBeans` object
whenever you stop using the TextView (it's detaching from the view tree, or the
container Activity or Fragment is going in paused state, ...).

This allows a deeper cleanup than what the `JumpingBeans` library is trying to
perform if you forget to. **Don't leave stuff lying around if you can!**

## Also, a few caveats
Please note that you:

 * **Must not** try to change a jumping beans text in a textview before calling
   `stopJumping()` as to avoid unnecessary invalidation calls;
   the JumpingBeans class cannot know when this happens and will keep
   animating the textview (well, try to, anyway), wasting resources
 * **Must not** try to use a jumping beans text in another view; it will not
   animate. Just create another jumping beans animation for each new
   view
 * **Must not** use more than one JumpingBeans instance on a single TextView, as
   the first cleanup operation called on any of these JumpingBeans will also cleanup
   all other JumpingBeans' stuff. This is most likely not what you want to happen in
   some cases.
 * **Should not** use JumpingBeans on large chunks of text. Ideally this should
   be done on small views with just a few words. We've strived to make it as inexpensive
   as possible to use JumpingBeans but invalidating and possibly relayouting a large
   TextView can be pretty expensive.