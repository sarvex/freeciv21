.. SPDX-License-Identifier: GPL-3.0-or-later
.. SPDX-FileCopyrightText: Freeciv21 and Freeciv Contributors
.. SPDX-FileCopyrightText: James Robertson <jwrober@gmail.com>
.. SPDX-FileCopyrightText: Louis Moureaux <m_louis30@yahoo.com>

Citizen Governor (aka Citizen Management Agent, or CMA)
*******************************************************

The Citizen Governor is designed to help you manage your cities, i.e. deploy the workers on the available
tiles around (or make them scientists, taxmen, or even entertainers), to get the most out of the city. You can
switch the Governor on or off at any time for any city, but there are some handicaps (explained below), if you
have governor controlled and non-governor-controlled cities nearby.

The heart of the Governor system is an optimizing algorithm, that tries to deploy the workers of a city in
such a way, that a user-defined goal is achieved as much as possible. You know probably, there is already a
kind of optimizing, when you open a city, and click on the center tile (the city symbol) of the map. This
optimization tries to maximize mostly the food output, but it does not care about disorder.

The City Management Agent goes far beyond this old form of optimizing. First, it performs this task every time
anything changes with the city. If the city grows or shrinks, troops go in or out, tiles get irrigation or
mining, or are occupied by an enemy, the Governor becomes active. Second, it supports all kinds of optimizing,
like production (shields), gold, science, or luxury goods. Third, it gives the player a fine-grained control
over this, with the possibility of setting constraints for any kind of city output. The latter includes the
constraint of celebration, which makes it very easy to let your cities grow, even in harder times. The forth,
and probably most valuable thing in war times, is that is keeps your cities content, preventing them from
revolt.

The legacy Freeciv Wiki also contains some other useful information related to the CMA:

* https://freeciv.fandom.com/wiki/City_manager
* https://freeciv.fandom.com/wiki/CMA

Usage
=====

You can set up the Governor for a city by opening the :doc:`City Dialog </Manuals/Game/city-dialog>` and
clicking on the :guilabel:`Governor` tab. You can choose a preset for a specified goal in the middle and on
the bottom you can specify more complex goals by moving the sliders. You can choose a preset at first, and
then modify it. Once you have created a new setting, you can add a preset name for it. This is not required,
but is very useful, since you can watch and even change the city's setting from within the city view, if it is
given a name. Do not forget to save settings (in the :guilabel:`Game` menu), when you have created new
presets.

.. _CMA Dialog:
.. figure:: /_static/images/gui-elements/city-dialog-governor.png
    :align: center
    :alt: Governor Tab in City Dialog
    :figclass: align-center

    Governor Tab in City Dialog


The sliders are of two kinds: the rightmost sliders are factors, which gauges how much one product is worth
compared to the others (e.g how much shields are worth with respect to everything else). The leftmost sliders
are constraints. You can command the city not to lose food, e.g. by setting the surplus constraint to zero,
and you can allow the city to lose gold by setting the gold surplus to -3, and urge them to make at least 5
shields per round by setting the production surplus to 5. The most powerful constraint, though, is the
Celebrate constraint, which makes the city celebrate at once (which usually takes effect the round after you
change it).

It is obvious that the Governor cannot fulfill all these constraints in every case. Whenever the constraints
cannot be fulfilled, the Governor quits its service for that city, giving a message:
:strong:`"The agent can't fulfill the requirements for Berlin. Passing back control."` You then have the
choice of either managing the city on your own (which has some drawbacks, see below), or open that city and
change the surplus requirements so that they can be fulfilled.

When you have made a setup for a city, you need to click on :guilabel:`Enable` to switch on the Governor. If
this button's text is greyed, either the Governor is already active, or the task is impossible. In the
latter case you see dashes instead of numbers in the results block. If you ever want to switch off the
Governor deliberately, click :guilabel:`Disable`.

Advanced Usage
==============

Usually the goal(s) of your cities depend on the phase your game is in, whether you want to spread widely,
grow quickly, research advanced techs or wage war. You may want to set up a high factor for science to
research, or a high shields factor to produce units. The highest factor available is 25, that means: if the
shields factor is set to 25, and other to 1, the Governor prefers a single shield over 25 gold (or trade
also). This is pretty much because money can buy units too. That also means that the Governor is indifferent
about producing gold, science, luxury goods, or food, but when you wage war, you usually prefer gold or
luxury goods. So it is probably a good idea to set a second (or even third) preference for the city's output,
e.g. gold factor 5. That still prefers 1 shield over 5 gold (and 1 gold over 5 food or anything else).

Constraints are not useful in all cases. If you want a high income, it is probably better to set the gold
factor to 25, than to set a minimal surplus of 5 or so. Because a big city can make more gold than a small
one, you would end up setting a different surplus for each city.

However, if the shields surplus of a city is below zero, it cannot support all of its units any more. You
will lose some of the units the city supports. If the food surplus is negative, the city will starve and
eventually (when the granary is empty) shrink. This may be intended, but if the city supports any settlers,
you will lose them before the city shrinks. Constraints can also have a warning function.

Which constraints can be fulfilled depends widely on the global science, tax, and luxury good rates. E.g. a
gold surplus >= 0 is easier to fulfill with a higher tax rate than a lower one. You should always consider to
change these rates, when you going to change the Governor settings for the most of your cities.

Drawbacks
=========

The Governor is a very powerful tool, which not only releases you from the micromanagement of your cities,
but gives you more performance than you have ever seen (well, for most players).

There are some drawbacks, though. Once you have switched on the Governor, it grabs any good tile it can get.
So you encounter very hard times trying to manage a city nearby a Governor-controlled one. This is true for
the city dialog and the main map worker's interface as well. If you want to have Governor-controlled and
:strong:`handmade` cities, they probably should be on different islands.

There are several situations where the Governor cannot fulfill the requirements just temporarily, e.g. when
you move a ship from one city to another, or when an enemy walks through your country. The Governor passes
back control in these cases, and you have to reenable it manually. A general approach to prevent this might
be, to set the minimal surpluses as low as possible (-20). Of course you must be careful with the food and
shield surpluses.

While the Governor does a really good job for a single city, no tile will ever be released for the good of
another city. Also, the Governor controlled cities are computed in a more random order. The results may
depend on it and change, when a recalculation is done (e.g. when tax changes). So, no guarantee is given
that the overall results are always optimal.

Settings file
=============

The game allows the user to load and save preset parameters for the agent. Choosing
:menuselection:`Game --> Options --> Save Settings Now` will not only save your
:ref:`interface options <game-manual-options>` and :ref:`message options <game-manual-message-options>`, but
it will save any changes you made to you Governor presets as well.

The format for the options file (usually :file:`~/.local/share/freeciv21/freeciv-client-rc-X.Y` , where X.Y
is the version of freeciv21 in use) is as follows (in case you which to change these presets manually, i.e.
with a text editor).

Under the heading :literal:`[cma]`, is a :literal:`number_of_presets`. This should be set to the number of
presets that are present in the options file. If you manually add or remove a preset, you need to change
this number as appropriate.

After this, is an array that houses the presets. Here is the header:

.. code-block:: ini

    preset={ "name","minsurp0","factor0","minsurp1","factor1","minsurp2",
    "factor2","minsurp3","factor3","minsurp4","factor4","minsurp5",
    "factor5","reqhappy","factortarget","happyfactor"

so the order of the preset should be as follows:

* name of preset, minimal surplus 0, factor 0, ... ,
* require city to be happy, what the target should be [0,1],
* the happiness factor

Currently there are 6 surpluses and factors. They are:

* 0 = food
* 1 = production
* 2 = trade
* 3 = gold
* 4 = luxury goods
* 5 = science

Also currently, :literal:`factortarget` is not changeable within the client.

The array should be terminated with a squirely brace :literal:`}`.

Here are the 5 presets that come with Freeciv21 out of the box:

.. code-block:: ini

    "Very happy",0,10,0,5,0,0,-20,4,0,0,0,4,FALSE,25
    "Prefer food",-20,25,0,5,0,0,-20,4,0,0,0,4,FALSE,0
    "Prefer production",0,10,-20,25,0,0,-20,4,0,0,0,4,FALSE,0
    "Prefer gold",0,10,0,5,0,0,-20,25,0,0,0,4,FALSE,0
    "Prefer science",0,10,0,5,0,0,-20,4,0,0,0,25,FALSE,01

Here are 16 more that you can add to your client RC file:

.. code-block:: ini

    "+2 food",2,1,0,1,0,1,0,1,0,1,0,1,0,0,1
    "+2 production",0,1,2,1,0,1,0,1,0,1,0,1,0,0,1
    "+2 trade",0,1,0,1,2,1,0,1,0,1,0,1,0,0,1
    "+2 gold",0,1,0,1,0,1,2,1,0,1,0,1,0,0,1
    "+2 luxury",0,1,0,1,0,1,0,1,2,1,0,1,0,0,1
    "+2 science",0,1,0,1,0,1,0,1,0,1,2,1,0,0,1
    "+20 Celebrating for Gold",20,0,0,16,0,0,0,8,0,1,0,1,TRUE,0
    "Max food no gold limit",0,10,0,1,0,1,-20,1,0,1,0,1,0,0,1
    "Max production no gold limit",0,1,0,10,0,1,-20,1,0,1,0,1,0,0,1
    "Max trade no gold limit",0,1,0,1,0,10,-20,1,0,1,0,1,0,0,1
    "Max gold no gold limit",0,1,0,1,0,1,-20,10,0,1,0,1,0,0,1
    "Max luxury no gold limit",0,1,0,1,0,1,-20,1,0,10,0,1,0,0,1
    "Max science no gold limit",0,1,0,1,0,1,-20,1,0,1,0,10,0,0,1
    "Max food+prod. no gold limit",0,10,0,10,0,1,-20,1,0,1,0,1,0,0,1
    "Max food+prod.+trade",0,10,0,10,0,10,0,1,0,1,0,1,0,0,1
    "Max all",0,1,0,1,0,1,0,1,0,1,0,1,0,0,1

Here are 6 more that have been added as an afterthought:

.. code-block:: ini

    "+1 food, max prod. no gold limit",1,1,0,10,0,1,-20,1,0,1,0,1,0,0,1
    "+2 food, max prod. no gold limit",2,1,0,10,0,1,-20,1,0,1,0,1,0,0,1
    "+3 food, max prod. no gold limit",3,1,0,10,0,1,-20,1,0,1,0,1,0,0,1
    "+4 food, max prod. no gold limit",4,1,0,10,0,1,-20,1,0,1,0,1,0,0,1
    "+5 food, max prod. no gold limit",5,1,0,10,0,1,-20,1,0,1,0,1,0,0,1
    "+6 food, max prod. no gold limit",6,1,0,10,0,1,-20,1,0,1,0,1,0,0,1

and even more, some with multiple goals:

.. code-block:: ini

    "research at any cost",0,1,0,5,-20,1,-20,1,-20,1,-20,25,0,0,1
    "celebration and growing",1,1,0,25,-20,1,-20,12,-20,1,-20,1,1,0,1
    "grow at any cost",1,25,0,5,-20,1,-20,1,-20,1,-20,5,0,0,1
    "research and some shields",0,1,0,8,0,1,-3,1,0,1,0,25,0,0,1
    "shields and a bit money",0,1,0,25,0,1,-3,3,0,1,0,1,0,0,1
    "many shields and some money",0,1,0,25,0,1,0,9,0,1,0,1,0,0,1
    "shields and some research",0,1,0,25,0,1,-2,1,0,1,0,8,0,0,1
    "celebrate and grow at once",1,1,0,25,-20,1,-20,1,-20,1,-20,8,1,0,1

Enjoy using your citizen Governors!
