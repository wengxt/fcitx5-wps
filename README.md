A easy way to build fcitx5 im module for WPS.

This is basically the Qt4's BUILD\_ONLY\_PLUGIN Mode, but with one difference.
It doesn't link to Qt library, so it doesn not need to deal with the different
library layout between WPS's Qt and standard Qt. (WPS merged libQtGui into
libQtCore)

While the fcitx5-qt has two different licenses, this only builds the 3-BSD part
of fcitx5-qt (the im module + dbus).
