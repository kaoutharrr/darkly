# Hidden Directory - README

## Description

The file `/robots.txt` revealed a hidden directory:

```bash
/.hidden/
```

Normally, `robots.txt` tells search engines which paths should not be indexed, but in this case it accidentally exposed a sensitive location instead of protecting it.

---

## Directory Structure

Inside `/.hidden/` there was a recursive maze:

* 26 folders (`a` to `z`)
* each folder contained more subfolders
* each subfolder contained a `README`
* manual browsing was impractical

Example:

```bash
http://192.168.56.6/.hidden/README
```

Output:

```text
Tu veux de l'aide ? Moi aussi !
```

---

## Automated Exploration

To avoid manual navigation, all `README` files were downloaded automatically:

```bash
wget -r -np -l 10 --accept "README" -e robots=off \
http://192.168.56.6/.hidden/ -P /tmp/hidden
```

### Command explanation

* `-r` → recursive download
* `-np` → do not go to parent directories
* `-l 10` → maximum depth 10
* `--accept "README"` → download only README files
* `-e robots=off` → ignore robots.txt rules

---

## Extracting the Flag

After downloading, all README contents were searched:

```bash
find /tmp/hidden -name "README" | xargs grep -h "" | sort -u
```

This command:

1. finds all README files
2. reads their content
3. removes duplicates
4. displays unique messages

Output included:

```text
Demande à ton voisin de droite
Demande à ton voisin de gauche
Demande à ton voisin du dessous
Demande à ton voisin du dessus
Hey, here is your flag : d5eec3ec36cf80dce44a896f961c1831a05526ec215693c8f2c39543497d4466
```

---

## Flag

```text
d5eec3ec36cf80dce44a896f961c1831a05526ec215693c8f2c39543497d4466
```

---

## Vulnerability

This level demonstrates:

**Sensitive Information Disclosure**

Because:

* `robots.txt` exposed a hidden path
* hidden files were publicly accessible
* no access control protected the directory
