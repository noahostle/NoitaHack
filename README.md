# NoitaHack
Noita Dll injection / memory editor written in c by yours truly.

I am using cheat engine to find signatures / pointers and then I will be writing a Dll injector, and a Dll to perform memory edits and to hook functions of interest.

These pointers only work when you select "continue" from the main menu which is a little annoying, maybe I could swap from pointers to function sigs instead?




Current things reverse engineered:


# Todo:

Mana: (Float) but is tied to individual wands, I did manage to find a list of wands in memory at one point, but only one of them seemed to update when I fired it,
and when i found a couple static pointers to it they were all pointing into garbage upon restart.

Re-find velocity signature and make sure its valid (and find noppable range)

Damage hack

Wand editing

Perk editing

Orb editing?

Item Gib

Aimbot (maybe? dont know how useful this would be)

Night vision (perhaps use all seeing eye perk)

Write the actual injector and Dll

Auto restart / continue after making new game (or switch pointers to sigs)



# Pointers:

"noita.exe"+00E05718
+0
+28
+70
\___
    Player Base !!!
    \___
        + 20 > + 110 --> levi    (float, 0.0 - 3.0)
        + 2C > + 48  --> hp      (double, 4% of actual hp)
        + 2C > + 50  --> maxhp   (double, 4% of actual maxhp)
        + F8 > + 48  --> cash    (4 byte int, true to val)
	
	      + F4 > + 6C  --> xVel    (float, -1 = left)
	      + F4 > + 70  --> yVel    (float, -1 = up)
       // these two ^ are not writeable by cheat engine
       // fast enough to keep you from jittering (see sigs)




"noita.exe"+00E20470
\___
    + 50 --> playerX   (float, -1 = left)
    + 54 --> playerY   (float, -1 = up)




noita.exe+E1FBB8
\___
    mouseX    (float)

noita.exe+E1FBBC
\___
    mouseY    (float)

Could use to make aimbot once
I find monster entity list


Mouse notes:
x: 0-left 1279-right
y: 0-top  719-bottom
x-640, y-360
x: -640 left, 640 right
y: -360 top, 360 bottom

tp2cursor func:
xpos=xpos+(mousex-640)*.44
ypos=ypos+(mousey-360)*.44





# Signatures:


Update Player velcocity function sig:
\[ F3 0F 10 07 F3 0F 5C 46 50 F3 0F 11 45 F8 F3 0F 10 47 04 F3 0F 5C 46 54 8B 45 F8 89 41 6C F3 0F 11 45 FC 8B 45 FC 89 41 70 8B 07 89 46 50 8B 47 04 89 46 54 5F 5E 8B E5 5D \]
I accidentally died in a wall and had to start
new game after finding this.

Nopping some parts of this function did work,
Im just not sure if these bytes ^ are the ones
you should nop, or if they are just the sig.


            
Continue Game function sig:
CC CC CC 55 8B EC 56 8B 75 08 B9

maybe i could do, 
if mouseX not between etc, then pointers
are invalid and it is newgame, so
sys exit, start noita.exe again and
reinject, and call continue??
