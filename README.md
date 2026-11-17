# Picard plugins

Three plugins for [MusicBrainz Picard](https://picard.musicbrainz.org/) 2.x.
They were written alongside [Fusion](https://github.com/FooThemes/foo_fusion),
the foobar2000 skin, but work on their own.

| Plugin | What it does |
|---|---|
| **Language Variants** | Original, English and romanised versions of titles, artists and albums |
| **Genre Tidy** | One spelling per genre across the library |
| **Only These Tags** | Picard writes only the tags you list |

## Install

Download the files from [Releases](../../releases), then in Picard go to
**Options → Plugins → Install plugin…** and pick:

- `language_variants.zip` for Language Variants
- `genre_tidy.py` for Genre Tidy
- `only_these_tags.py` for Only These Tags

Each plugin's settings are under **Options → Plugins** once it's enabled.

## Language Variants

For releases that MusicBrainz links to translated or transliterated
pseudo-releases, it writes `TITLE_ORIG`, `TITLE_EN` and `TITLE_ROM`, and the
same for `ARTIST`, `ALBUM`, `ALBUMARTIST` and `DISCSUBTITLE`. It then rebuilds
the main tags from them in a format you choose per tag, such as
`English (原題)` for albums and `原題 (English)` for tracks.

- English artist names also come from the artists' English aliases.
- When MusicBrainz has no English version, it can machine translate one with
  DeepL or Google Cloud Translation, using your own API key. Off by default.
- When it has no romanised version, it can make one for Japanese, Chinese,
  Korean, Cyrillic and Greek. MusicBrainz versions always come first.

Fusion shows these tags and lets you pick which version leads.

## Genre Tidy

Standardises genres as they're saved, so "jpop", "J Pop" and "j-pop" all
become "J-Pop", "Rock Music" becomes "Rock", and duplicates go. It works on
what's actually about to be written, after MusicBrainz, scripts and other
plugins (Wikidata Genre, Last.fm) have added theirs.

You set the spellings to use, `from=to` replacements (wildcards or regular
expressions; an empty `to` removes the genre), and whether to drop a trailing
" music" and capitalise genres it has no spelling for.

## Only These Tags

While it's enabled, Picard writes only the tags you list. Every other tag in
the file stays exactly as it was, whatever "Clear existing tags", scripts,
other plugins or MusicBrainz say. Names are Picard's (`genre`, `mood`,
`musicbrainz_*` …), and `coverart` in the list lets it change embedded art.
It can also stop files being renamed or moved, and cover files being saved,
so a pass changes nothing but those tags.

## Building

Language Variants is a zip because it bundles the libraries it romanises
with. To build it:

```
python build.py
```

That downloads pykakasi, jaconv and pypinyin from PyPI at pinned versions,
unpacks them (nothing is run) and writes `dist/language_variants.zip`. The
other two plugins are single files and need no build.

## Releasing

Set `PLUGIN_VERSION` in the plugins that changed, commit, then tag and push
(`git tag v1.0.0`, `git push origin v1.0.0`), or make the release on GitHub's
site. The Release workflow builds the zip and attaches all three files.

## Licence

Each plugin carries its own licence in its header: Language Variants is
GPL-3.0-or-later (it bundles pykakasi, which is GPL-3.0-or-later; jaconv and
pypinyin are MIT), and Genre Tidy and Only These Tags are GPL-2.0-or-later.
