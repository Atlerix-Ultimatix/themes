# Proposer un thème / Proposing a theme

Un dossier par thème / one folder per theme:

```
themes/<id>/
  <id>.atlas-theme.json
  apercu.png      capture de l'application sous ce thème / screenshot of the app in this theme
  LISEZMOI.md     nom, auteur, licence, intention / name, author, licence, intent
```

`<id>` : minuscules, chiffres, tirets / lowercase letters, digits, hyphens.

1. **Le vérificateur dit « OK » / the checker says "OK"**:
   `node check-theme.mjs themes/<id>/<id>.atlas-theme.json`. L'automatisation du dépôt le relance
   sur chaque proposition / the repository's automation re-runs it on every proposal.
2. **Licence libre / free licence** dans `licence` et `LISEZMOI.md` (`CC0-1.0`, `CC-BY-4.0`).
3. **Thème de fans / fan theme**: le nom de l'œuvre est permis dans `nom` ; **toutes les images
   sont originales** (aucun sprite, logo, capture ou illustration officiels) ; aucun logo ni
   lettrage imité ; jamais présenté comme officiel ; `licence` porte « Thème de fans non officiel »
   / "Unofficial fan theme". The work's name may appear in `nom`; **all artwork is original**; no
   imitated logo or lettering; never presented as official.
4. **Une capture réelle / a real screenshot** `apercu.png`.
5. Pas de contenu choquant ni de nom de personne réelle / no shocking content, no real person's name.

Un thème signalé par un titulaire de droits est retiré sans discussion / a theme reported by a
rights holder is removed without discussion.
