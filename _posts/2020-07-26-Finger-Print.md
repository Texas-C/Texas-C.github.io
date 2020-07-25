---
title: Thinkpad X390 + Archlinux finger printer configs
published: true
---

# 0 prepare: update firmware & install fprint
upgrade firmware with `fwupd`

	fwupd update

install `fprint`

	sudo pacman -S fprint

# 1 config:

add the line `auth sufficient pam_fprintd.so` at the beginning of `/etc/pam.d/sddm` & `/etc/pam.d/kde`

notes: this line could be add to other configs `/etc/pam.d/{login,su,sudo,gdm,lightdm}`

`/etc/pam.d/sudo`

	auth		sufficient  	pam_unix.so try_first_pass likeauth nullok
	auth		sufficient  	pam_fprintd.so


# 2 start:

Start `fprintd`:

	sudo systemctl start fprintd

Enroll one of finger:

	fprintd-enroll

Then put one of finger on the finger-print sensor to record signature.

Enroll all fingers:

	for finger in {left,right}-{thumb,{index,middle,ring,little}-finger}; do fprintd-enroll -f "$finger" "$USER"; done

Then put each fingers on the finger-print sensor to record signatures.

Verify:

	fprintd-verify

Delete:

	fprintd-delete ${USER}

__Reference:__
[fwupd wiki](https://wiki.archlinux.org/index.php/Fwupd)
[fprint wiki](https://wiki.archlinux.org/index.php/fprint)
[sddm wiki](https://wiki.archlinux.org/index.php/sddm)
