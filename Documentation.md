# Smart Shoe
### By Jason Bellerjeau

[Live application](https://smart-object-ogbg2yi4r-jason-5ea7.vercel.app) | [Demo video](Screen_Recording_20261006_092201_Chrome.mp4)

## Overview

This project is a mock-up of the interface for a smart high-top shoe. The shoe has an inner lining made of air pockets. A button on the toe fills those pockets so the shoe forms around the wearer's foot, which takes the place of laces. A button on the heel lets the air back out. A zipper runs around the ankle, and once it is open the high top can fold down into a low top.

The page has two parts. The left side is the shoe and its controls. The right side has the project information, an info button, and buttons that simulate someone putting the shoe on so the interface has something to respond to.

### Affordances

A shoe is small and portable, but you don't hold it in your hand. It sits on the floor or on a foot, and it comes as a pair. When someone is wearing it, the only parts they can reach easily are the toe, the heel, and the ankle, and only by bending down. The sole can't be seen or touched at all.

Shoes also take a beating. They bend with every step, get wet, and scrape against the ground. That makes a screen a bad fit for most of the shoe. Physical buttons in spots that don't flex much hold up better and can be found by feel.

The design uses these surfaces:

- **Toe:** a pressable button that inflates the lining. The toe cap is stiff, so a button there won't get pressed by accident while walking.
- **Heel:** a pressable button that releases the air. The heel counter is also stiff, and it is the part people already grab when pulling a shoe off.
- **Ankle:** an unzippable section that lets the high top fold down. Zippers are something people already know how to use without being told.
- **Side of the sole:** a row of five lights that show how full the lining is. This is the one indicator on the shoe, and it sits where it can be seen by glancing down.
By placing the 2 buttons at the toe and heel, it allows for the user to press the buttons by clicking or kicking, making the process hands free.

## What is a "smart shoe"

My assumptions about what the shoe can sense and do:

- The shoe can tell when a foot is inside it.
- The lining is a set of air pockets that a small pump in the shoe can fill.
- The shoe knows how full the lining is and stops inflating on its own once it reaches a snug fit.
- The shoe can tell whether the zipper is closed and whether the top is folded up or down.

I'm not describing how these would be built, only what the interface depends on.

## Early Sketches

These are my first sketches. Some of the ideas here are self-tying laces, a back zipper, a companion phone app with exercise information, a top that flips down or up for more or less support, and a pad you step onto that makes the support form over your foot.

![Early sketches](images/image2.jpg)

[10-plus-10 sketches, storyboard, and hybrid sketch if they are separate from the page above]

## User Needs

### Interviews

I interviewed three people outside the class. The questions I asked were:

1. What do you wish your shoes did that they don't?
2. How active are you?
3. What do you think of when you hear "smart shoe"?
4. How many pairs of shoes do you own, and why?

![Interview questions](images/image1.jpg)

![Interview answers](images/image3.jpg)

**Participant 1** wanted shoes that tie themselves and fit to the size of the foot by expanding. They are moderately active. When they hear "smart shoe" they think of the self-lacing shoes from *Back to the Future*. They own 7 or 8 pairs, each used for something different.

**Participant 2** wanted shoes that track their steps. They are moderately active. To them a smart shoe is something that tracks steps and ties itself. They own 5 pairs, each for a different use, such as mowing, sandals, and dress shoes.

**Participant 3** wanted shoes that stay comfortable for longer. They are active. To them a smart shoe is well engineered and comfortable. They own more than 10 pairs, each for a different use.

Two things came up more than once. Two of the three people brought up self-tying or self-fitting shoes, and all three own several pairs because each pair only covers one use. Step tracking came from only one person.

### Needs and requirements

| User need | Design requirement |
| --- | --- |
| The user wants a shoe that tightens itself without laces | The toe button inflates the lining until it forms to the foot |
| The user wants a shoe that fits the size of their foot and stays comfortable for a long time | The air lining fills around the foot and stops at a snug fit instead of squeezing as tight as it can |
| The user owns many pairs because each one only covers one use | The ankle zipper lets the high top fold down, so one shoe works as a high top or a low top |
| The user needs to get the shoe off without untying anything | The heel button releases the air |
| The user needs to know how tight the shoe is | Lights on the sole show how full the lining is |
| The user shouldn't be able to put the shoe in a bad state | The shoe won't inflate unless a foot is in, the zipper is closed, and the top is up |

### Feedback on the vanilla sketch

[Feedback from the three people you showed the sketch to]

## Finalizing Ideas

The final design came from combining two of the early sketches. The top that flips down became the zipper and fold-down top, and the support that forms over the foot became the air lining. I dropped the self-tying laces because the air lining does the same job with fewer moving parts. I also dropped the phone app because I wanted all the controls to be on the shoe itself.

![Final design sketch](images/20261003_202426.jpg)

## The Interface

### Controls on the shoe

| Control | Location | What it does |
| --- | --- | --- |
| Inflate button (+) | Toe | Fills the air lining until it reaches a snug fit |
| Release button (−) | Heel | Lets all the air out of the lining |
| Unzip / Zip | Ankle | Opens or closes the zipper around the ankle |
| Fold down / Fold up | High top | Folds the top down into a low top, or back up |

### Indicators

- **Fill lights:** five lights on the side of the sole. Each one turns on as the lining fills, and four are lit at a snug fit.
- **Status list:** shows whether a foot is in, how full the lining is, whether the zipper is open, and whether the top is up or down.
- **Message box:** says what just happened, or why an action didn't work.

### Simulation controls

- **Info:** shows how to use the simulation.
- **Put foot in / Take foot out:** simulates someone putting the shoe on or taking it off.
- **Reset:** puts the shoe back to empty, zipped, and folded up.

### Rules

The controls depend on each other, so the shoe blocks actions that wouldn't make sense on a real shoe and says why in the message box:

- The lining only inflates when a foot is in the shoe, the zipper is closed, and the top is folded up.
- The zipper can't be opened while there is air in the lining.
- The top can only fold down after the zipper is open.
- The zipper can't be closed while the top is folded down.
- The foot can't be taken out until the air is released.

To go from a high top to a low top, the wearer releases the air, unzips, and folds the top down.

When the wearer puts their foot in and presses the toe button, the lining fills and the lights turn on one at a time. It stops at 70% and the message box says "Snug fit reached."

## Implementation

The project is built with Svelte 5 and Vite and hosted on Vercel. There are two components:

- **`App.svelte`** holds all the state (foot in, lining fill level, zipper, top position, current message) and the functions that run when a button is pressed. Each function checks the rules above before changing anything. Inflating and releasing use a `setInterval` that changes the fill level a little at a time so the lights fill up gradually instead of all at once.
- **`ShoeGraphic.svelte`** draws the shoe as an SVG. It takes `folded` as a prop and draws either the high top or the folded-down top.

The buttons on the shoe are HTML buttons positioned on top of the SVG with percentages, so the shoe drawing can be swapped for a photo later and the buttons only need to be moved.

## Future Work

- Replace the drawn shoe with a real photo of a high-top shoe.
- Let the wearer choose how tight a snug fit is instead of always stopping at 70%.
- Add the second shoe, since shoes come in pairs and each one would need its own buttons.
- Add step tracking, which one interview participant asked for. This would need a display or a companion app, since the shoe only has the fill lights.
- Simulate the shoe over a day of use, such as air slowly leaking and the shoe topping itself back up.

# Disclaimer
If any photos do not appear correctly, they are accessible via the file explorer