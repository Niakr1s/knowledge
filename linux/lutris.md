# Lutris

## Post install

After install open `Preferences` at `Runners` section and check whether `Wine` is enabled.
Then, in the left column of main window, click on button `Manage versions` near `Wine` and download appropriate wine version, for example, `lutris-ge`.

## Adding a game

If you want to add already installed game, click `+` in main window and choose `Add locally installed game`.
Set name and set `Runner` to `Wine`.
At `Game options` tab set game executable and wine directory (for example, `~/.wineprefixes/wine-ge`).
If there no prefix at that directory, `Lutris` will create it.
In `Runner options` select wine version.
Click `Save`.

## Enabling hud

Click `configure` (you can configure `wine` or each game separately), turn on `Advanced` switch and go to `System options`.

- [mangohud](https://github.com/flightlessmango/MangoHud): set `Command prefix` to `mangohud`.
You can use configure `mangohud` with [goverlay](https://github.com/benjamimgois/goverlay).
- [vkBasalt](https://github.com/DadSchoorse/vkBasalt): set `ENABLE_VKBASALT` to 1 in `Environment variables` section.
Default config for `vkBasalt` contained at `/etc/vkBasalt.conf`.
You can copy it to `~/.config/vkBasalt/vkBasalt.conf` and tweak it.
