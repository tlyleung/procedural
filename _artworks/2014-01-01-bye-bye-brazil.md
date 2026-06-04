---
layout: artwork
artist: marissa-rohr
title: Bye Bye Brazil
medium: Household gloss paint on canvas
dimensions: 15 ¾ × 19 ⅝ inches (40 × 50 cm)
p5: true
image: /assets/images/artworks/bye-bye-brazil/gallery-sm.jpg
---

<script>
const WIDTH = 800;
const HEIGHT = 1000;
const SCALE = 0.8;
const RENDERER = 'P2D';
const SEED = 0;

const COLORS = [
  '#1d1d1d',  // black
  '#ebebeb',  // white
  '#f28178',  // coral
  '#c62301',  // red
  '#d9e117',  // yellow
  '#a9dec1',  // mint
  '#a0d5e3',  // baby blue
  '#2b294c',  // dark blue
  '#7bccb8',  // green
  '#e7d7cd',  // light pink
  '#edc7b7',  // dark pink
  '#b4b4b4',  // grey
  '#730747',  // purple
  '#f0b600',  // orange
  '#92aadf',  // blue
  '#283b2b',  // dark green
  '#a55223',  // brown
];

function preload() {
  seed = SEED;
}

function sketch() {
  canvas.elt.setAttribute("title", `Seed: ${seed}`);

  pg.clear();
  
  for (let i = 0; i < 800; i += 100) {
    for (let j = 0; j < 1000; j += 100) {
      pg.strokeWeight(6);
      pg.stroke('#ebebeb');
      pg.fill(random(COLORS));
      pg.square(i, j, 100);

      let x = random();
      let gutter = 3;

      pg.strokeWeight(10);
      pg.fill(random(COLORS));
      let diameter = 200 - 10 - 2 * gutter;

      if (x < 0.15) {
        pg.arc(i + gutter, j + gutter, diameter, diameter, 0, HALF_PI);
      } else if (x < 0.30) {
        pg.arc(i + 100 - gutter, j + gutter, diameter, diameter, HALF_PI, PI);
      } else if (x < 0.45) {
        pg.arc(i + 100 - gutter, j + 100 - gutter, diameter, diameter, PI, PI + HALF_PI);
      } else if (x < 0.60) {
        pg.arc(i + gutter, j + 100 - gutter, diameter, diameter, PI + HALF_PI, TWO_PI);
      }
    }
  }

  image(pg, 0, 0, WIDTH, HEIGHT);
}
</script>
