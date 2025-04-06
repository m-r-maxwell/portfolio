+++
title = 'Networking'
date = 2025-04-067T12:00:41-04:00
draft = false
+++

## A new exploration

When I was in middle school I played an online game called runescape, turns out the game is still around but a bit changed. There are two versions, the newer modern version and the old school version that is like the version that I played. <br>
Giving in to nostalgia, I started playing it again recently but when playing the game since I'm in my 30s I want to be effienct while playing right? Or at the very least I want to earn money in game while training skills.

## New CLI App
### Ge Price Look Up

Since I am primarily working with python professionally at the moment I thought it would be a good idea to explore some different options in python. So the main components used here are...

- Python Modules for the Skills listed below
- Rich for text formatting and tabular output as well as colored output red for loss and green for profit
- PyInstaller for creating a binary executable
- API Provided by jagex, the maker of OSRS

Side note: I was very surprised to find out that `match case` support is relatively new in python.

```txt
What would you like to find the profit for?
1: Potion making
2: Herb cleaning via degrime
3: Cooking
4: Fletching
5: Smithing
6: Magic
7: All
8: Exit

# The formatting here got a little weird but does not show up like this in the terminal.
Enter your choice: 5
     Smithing Profits (Per Bar)
┏━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━┓
┃ Item             ┃ Profit per Bar ┃
┡━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━┩
│ Cannonball       │         1408gp │
├──────────────────┼────────────────┤
│ Iron dart tip    │          264gp │
├──────────────────┼────────────────┤
│ Bronze dart tip  │          261gp │
├──────────────────┼────────────────┤
│ Steel dart tip   │          242gp │
├──────────────────┼────────────────┤
│ Mithril dart tip │          229gp │
├──────────────────┼────────────────┤
│ Adamant dart tip │          169gp │
├──────────────────┼────────────────┤
│ Rune dart tip    │          130gp │
└──────────────────┴────────────────┘
```

It was a fun foray into some different parts of python than what I work on professionally, and seeing the different options of being able to convert a python script from a .py file to a executable was neat, since there are a number of options out there. I tried nuitka as well and understand that it will give much better improvements over PyInstaller but for ease of use that is what I decided to go with.