# Fonts

## Void linux

Use this command to disable bitmap fonts:

```bash
ln -s /usr/share/fontconfig/conf.avail/70-no-bitmaps.conf /etc/fonts/conf.d/
xbps-reconfigure -f fontconfig
```

I prefer these font packages:

- noto-fonts-cjk: To turn on display of China, Japanese, Korean symbols.
- noto-fonts-emoji: To turn on display of emoji.
- noto-fonts-ttf: Main fonts for UI.
- font-firacode: Font for terminal and editors.

Sometimes when you install additional font, there can be some shit with ui font.
For example, Firefoxe's menu can be blurry.
I don't know how to fix it, so just remove that font.
