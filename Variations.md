## Catalog of Variations ##

The variations are used as follows:

  * An initial affine transformation of the input point (x,y) is done using the 'coefs' part of the transform. This results in a new set of coordinates, (X,Y).
  * The (X,Y) coordinates are used to feed each of the different variations selected in the xform.
  * The contribution of each weighted variation is added together to generate a third set of coordinates, (X',Y').
  * If there is a post transform present in the xform, an additional affine transform based on the 'post' coefs in the xform is performed on (X',Y').

While most of the variations rely solely on the input coordinates (X,Y) for their output, some are 'parameterized' as  noted.

(X,Y) will be used for the input coordinates. W will be used for the variation weight. (Vx,Vy) will be used for post-variation coordinates.


---


(0) Linear
```
Vx = W * X;
Vy = W * Y;
```


---


(1) Sinusoidal
```
Vx = W * sin(X);
Vy = W * sin(Y);
```


---


(2) Spherical
```
r2 = 1 / (X^2 + Y^2 + 1e-6);

Vx = W * X * r2;
Vy = W * Y * r2;
```

---


(3) Swirl
```
r2 = X^2 + Y^2;
c1 = sin(r2);
c2 = cos(r2);

Vx = W * (c1 * X - c2 * Y);
Vy = W * (c2 * X + c1 * Y);
```

---


(4) Horseshoe
```
r  = 1 / (sqrt(X^2 + Y^2) + EPS);

Vx = W * (X-Y) * (X+Y) * r;
Vy = W * 2 * X * Y * r;
```

---


(5) Polar
```
r  = sqrt(X^2 + Y^2);
a  = atan2(X,Y);

Vx = W * a / PI;
Vy = W * (r - 1.0);
```

---


(6) Handkerchief
```
r  = sqrt(X^2 + Y^2);
a  = atan2(X,Y);

Vx = W * r * sin(a+r);
Vy = W * r * cos(a-r);
```

---


(7) Heart
```
a  = atan2(X,Y) * sqrt(X^2 + Y^2);
r  = W * sqrt(X^2 + Y^2);

Vx = r * sin(a);
Vy = (-r) * cos(a);
```

---


(8) Disc
```
a  = atan2(X*PI,Y*PI) / PI;
r  = PI * sqrt(X^2 + Y^2);

Vx = W * sin(r) * a;
Vy = W * cos(r) * a;
```

---


(9) Spiral
```
r  = sqrt(X^2 + Y^2) + 1e-6;
a  = atan2(X,Y);
r1 = W / r;

Vx = r1 * (cos(a) + sin(r));
Vy = r1 * (sin(a) - cos(r));
```

---


(10) Hyperbolic
```
r  = sqrt(X^2 + Y^2) + 1e-6;
a  = atan2(X,Y);

Vx = W * sin(a) / r;
Vy = W * cos(a) * r;
```

---


(11) Diamond
```
r  = sqrt(X^2 + Y^2);
a  = atan2(X,Y);

Vx = W * sin(a) * cos(r);
Vy = W * cos(a) * sin(r);
```

---


(12) Ex
```
a  = atan2(X,Y);
r  = sqrt(X^2 + Y^2);
n0 = sin(a+r);
n1 = cos(a-r);
m0 = r * n0^3;
m1 = r * n1^3;

Vx = W * (m0 + m1);
Vy = W * (m0 - m1);
```

---


(13) Julia
```
a  = atan2(X,Y)/2;
if ( random_bit )
  a += PI;
r  = W * (X^2 + Y^2)^0.25;

Vx = r * cos(a);
Vy = r * sin(a);
```

---


(14) Bent
```
if (X < 0)   X *= 2.0;
if (Y < 0)   Y /= 2.0;

Vx = W * x;
Vy = W * y;
```

---


(15) Waves
```
nx = X + c(1,0) * sin(Y / (c(2,0)^2 + EPS));
ny = Y + c(1,1) * sin(X / (c(2,1)^2 + EPS));

Vx = W * nx;
Vy = W * ny;
```

---


(16) Fisheye
```
r = sqrt(X^2 + Y^2);
K = 2 * W / (r+1);

Vx = K * y;
Vy = K * x;
```

---


(17) Popcorn
```
dx = tan(3*Y);
dy = tan(3*X);
nx = X + c(2,0) * sin(dx);
ny = Y + c(2,1) * sin(dy);

Vx = W * nx;
Vy = W * ny;
```

---


(18) Exponential
```
dx = W * e^(X-1);
dy = PI * Y;

Vx = dx * cos(dy);
Vy = dx * sin(dy);
```

---


(19) Power
```
a  = atan2(X,Y);
r  = W * (sqrt(X^2 + Y^2))^sin(a);

Vx = r * cos(a);
Vy = r * sin(a);
```

---


(20) Cosine
```
nx = cos(X*PI) * cosh(Y);
ny = -sin(X*PI) * sinh(Y);

Vx = W * nx;
Vy = W * ny;
```

---


(21) Rings
```
dx = c(2,0)^2 + EPS;
r  = sqrt(X^2 + Y^2);
a  = atan2(X,Y);
rp = W * (fmod(r+dx,2*dx) - dx + r*(1-dx));

Vx = rp * cos(a);
Vy = rp * sin(a);
```

---


(22) Fan
```
dx = PI * (c(2,0)^2 + EPS);
dy = c(2,1);
dx2 = dx/2;
a = atan2(X,Y);
r = W * sqrt(X^2 + Y^2);
a += (fmod(a+dy,dx) > dx2) ? -dx2 : dx2;

Vx = r * cos(a);
Vy = r * sin(a);
```

---


(23) Eyefish
```
r  = (2 * W) / (sqrt(X^2 + Y^2) + 1);

Vx = r * X;
Vy = r * Y;
```

---


(24) Bubble
```
r  = W / ( (X^2 + Y^2)/4 + 1);

Vx = r * X;
Vy = r * Y;
```

---


(25) Cylinder
```
Vx = r * sin(X);
Vy = r * Y;
```

---


(26) Noise
```
r  = W * random;
Vx =  X * r * cos(random * 2 * PI);
Vy = Y * r * sin(random * 2 * PI);
```

---


(27) Blur

```
r  = W * random;
Vx = r * cos(random * 2 * PI);
Vy = r * sin(random * 2 * PI);
```

---


(28) Rings2 (parameter: rings2\_val)
```
r  = sqrt(X^2 + Y^2);
a  = atan2(X,Y);
dx = rings2_val^2 + EPS;
r += -2*dx*(int)((r+dx)/(2*dx)) + r*(1-dx);

Vx = W * sin(a) * r;
Vy = W * cos(a) * r;
```

---


(29) Fan2 (parameters: fan2\_x, fan2\_y)
```
dx = PI * (fan2_x^2 + EPS);
dx2 = dx / 2;
a = atan2(X,Y);
r = W * sqrt(X^2 + Y^2);
t = a+fan2_y - dx * (int)((a+fan2_y)/dx);
if (t>dx2)
   a-=dx2;
else
   a+=dx2;

Vx = r * sin(a);
Vy = r * cos(a);
```

---


(30) Blob (parameters: blob\_high, blob\_low, blob\_waves)
```
r = sqrt(X^2 + Y^2);
a = atan2(X,Y);
bdiff = blob_high - blob_low;

r = r * (blob_low + bdiff * (1/2 + sin(blob_waves * a)/2));

Vx = W * sin(a) * r;
Vy = W * cos(a) * r;
```

---


(31) PDJ (parameters: pdj\_a, pdj\_b, pdj\_c, pdj\_d)
```
nx1 = cos(pdj_b * X);
nx2 = sin(pdj_c * X);
ny1 = sin(pdj_a * Y);
ny2 = cos(pdj_d * Y);

Vx  = W * (ny1 - nx1);
Vy  = W * (nx2 - ny2);
```

---


(32) Perspective (parameters: perspective\_angle, perspective\_dist)
```
vsin = sin(perspective_angle * PI / 2);
vfcos = W * perspective_dist * cos(perspective_angle * PI / 2);
t  = (perspective_dist - X * vsin);
Vx = vf * X / t;
Vy = vfcos * Y / t;
```

---


(33) JuliaN (parameters: julian\_power, julian\_dist)
```
rN = abs(julian_power);
cn = julian_dist / julian_power / 2;

sina = sin((arctan2(Y, X) + 2 * PI * random(rN)) / N);
cosa = cos((arctan2(Y, X) + 2 * PI * random(rN)) / N);

r = W * Power(X * X + Y * Y, cn);

Vx = r * cosa;
Vy = r * sina;
```

---


(34) JuliaScope (parameters: juliascope\_power, juliascope\_dist)
```
rN = abs(juliascope_power);
cn = juliascope_dist / juliascope_power / 2;

rnd = random(rN);

if ((rnd and 1) = 0) {
  sina = sin((2 * PI * rnd + arctan2(Y, X)) / juliascope_power);
  cosa = cos((2 * PI * rnd + arctan2(Y, X)) / juliascope_power);
} else {
  sina = sin((2 * PI * rnd - arctan2(Y, X)) / juliascope_power);
  cosa = cos((2 * PI * rnd - arctan2(Y, X)) / juliascope_power);
}

r = W * Power (X * X + Y * Y, cN);
Vx = r * cosa;
Vy = r * sina;
```

---


(35) Blur
```
tmpr = random * 2 * PI;
sinr = sin(tmpr);
cosr = cos(tmpr);

r = W * random;

Vx = r * cosr;
Vy = r * sinr;
```

---


(36) Gaussian
```
ang = random * 2 * PI;
sina = sin(ang);
cosa = cos(ang);

r = W * (random + random + random + random - 2.0);

Vx = r * cosr;
Vy = r * sinr;
```

---


(37) RadialBlur (parameter: radialBlur\_angle)
```
spinvar = sin(radialBlur_angle * PI / 2);
zoomvar = cos(radialBlur_angle * PI / 2);

rndG = W * (random + random + random + random - 2.0);
rA = sqrt(X^2 + Y^2);
tmpa = arctan2(Y, X) + spinvar*rndG;
sa = sin(tmpa);
ca = cos(tmpa);
rz = zoomvar * rndG - 1;

Vx = ra * ca + X*rz;
Vy = ra * sa + Y*rz;
```

---


(38) Pie (parameters: pie\_slices, pie\_rotation, pie\_thickness)
```
sl = (int) (random * pie_slices + 0.5);
a = pie_rotation + 2 * PI * (sl + random * pie_thickness) / pie_slices;
r = W * random;

Vx = r * cos(a);
Vy = r * sin(a);
```

---


(39) Ngon (parameters: ngon\_power, ngon\_sides, ngon\_corners, ngon\_circle)
```
r_factor = Power(X^2 + Y^2, ngon_power/2.0);
theta = ArcTan2(Y,X);
b = 2 * PI / ngon_sides;

phi = theta - (b * floor(theta / b));
if (phi > b/2)
   phi = phi - b;

amp = ngon_corners * (1.0 / (cos(phi) + EPS) - 1.0) + ngon_circle;
amp = amp / (r_factor + EPS);

Vx = W * X * amp;
Vy = W * Y * amp;
```

---


(40) Curl (parameters: curl\_c1, curl\_c2)
```
re = 1 + (c1 * X) + (c2 * X^2) - Y^2;
im = (c1 * Y) + (2 * c2 * X * Y);

r = W / (re^2 + im^2);

Vx = ((X * re) + (Y * im)) * r;
Vy = ((Y * re) - (X * im)) * r;
```

---


(41) Rectangles (parameters: rectangles\_x, rectangles\_y)
```
if rectangles_x==0
   Vx = W * X;
else
   Vx = W * ((2 * floor(X / rectangles_x) + 1) * rectangles_x - X);

if rectangles_y==0
   Vy = W * Y;
else
   Vy = W * ((2 * floor(Y / rectangles_y) + 1) * rectangles_y - Y);

```

---


(42) Arch
```
ang = random * W * PI;

Vx = W * sin(ang);
Vy = W * sin(ang)*sin(ang)/cos(ang);
```

---


(43) Tangent
```
Vx = W * sin(X)/cos(Y);
Vy = W * sin(Y)/cos(Y);
```

---


(44) Square
```
Vx = W * (random - 0.5);
Vy = W * (random - 0.5);
```

---


(45) Rays
```
ang = W * random * PI;
r = W / ( X^2 + Y^2 + EPS);
tanr = W * tan(ang) * r;

Vx = tanr * cos(X);
Vy = tanr * sin(Y);
```

---


(46) Blade
```
r = W * random * sqrt(X^2 + Y^2);

Vx = W * X * ( cos(r) + sin(r) );
Vy = W * X * ( cos(r) - sin(r) );
```

---


(47) Secant2
```
r = W * sqrt(X^2 + Y^2);

Vx = W * X;
if (cos(r)<0)
   Vy = W * (1.0/cos(r) + 1);
else
   Vy = W * (1.0/cos(r) - 1);
```

---


(48) Twintrian
```
r = W * random * sqrt(X^2 + Y^2);
sinr = sin(r);
cosr = cos(r);
diff = log10(sinr*sinr) + cosr;

Vx = W * X * diff;
Vy = W * X * (diff - sinr*PI);
```

---


(49) Cross
```
s = X^2-Y^2;
r = W * sqrt(1.0 / (s^2+EPS));

Vx = X * r;
Vy = Y * r;
```

---


(50) Disc2 (parameters: disc2\_rot, disc2\_twist)
```
d2rtp = disc2_rot * PI;
disc2_sinadd = sin(disc2_twist);
disc2_cosadd = cos(disc2_twist);

if (disc2_twist > 2*PI) {
   k = 1 + disc2_twist - 2*PI;
   disc2_sinadd *= k;
   disc2_cosadd *= k;
}

if (disc2_twist < -2 * PI) {
   k = 1 + disc2_twist + 2*PI;
   disc2_sinadd *= k;
   disc2_cosadd *= k;
}

t = disc2_rot * PI * (X + Y);
sinr = sin(t);
cosr = cos(t);
r = W * arctan2(X,Y) / PI;

Vx = (sinr + disc2_cosadd) * r;
Vy = (cosr + disc2_sinadd) * r;
```

---


(51) Super\_Shape (parameters: supershape\_m, supershape\_n1, supershape\_n2, supershape\_n3, supershape\_holes, supershape\_rnd)
```
pm_4 = supershape_m / 4.0;
pneg1_n1 = -1.0 / supershape_n1;

theta = pm_4 * arctan2(Y,X) + PI/4.0;
t1 = | sin(theta) |;
t2 = | cos(theta) |;

t1 = t1 ^ supershape_n2;
t2 = t2 ^ supershape_n3;

rnd = supershape_rnd;

r = W * ( (rnd*random + (1.0-rnd)*sqrt(X^2+Y^2) - supershape_holes) * (t1+t2)^pneg1_n1 ) / sqrt(X^2+Y^2);

Vx = r * X;
Yy = r * Y;


```

---


(52) Flower (parameters: flower\_holes, flower\_petals)
```
theta = arctan2(Y,X);
r = W * (random - flower_holes) * cos(flower_petals * theta);

Vx = r * cos(theta);
Vy = r * sin(theta);
```

---


(53) Conic (parameters: conic\_holes, conic\_eccen)
```
theta = arctan2(Y,X);
r = W * (random - conic_holes) * conic_eccen / (1 + conic_eccen * cos(theta));

Vx = r * cos(theta);
Vy = r * sin(theta);
```

---


(54) Parabola (parameters: parabola\_height, parabola\_width)
```
r = sqrt(X^2 + Y^2);

Vx = W * parabola_height * sin(r) * sin(r) * random;
Vy = W * parabola_width * cos(r) * random;
```

---

Flam3 2.8 Variations

---


(55) Bent2 (parameters: bent2\_x, bent2\_y)
```
if (X < 0)   X *= bent2_x;
if (Y < 0)   Y *= bent2_y;

Vx = W * X;
Vy = W * Y;
```