![Tests](https://github.com/mmasdeu/materials_eines/actions/workflows/makeall.yml/badge.svg)

# Material per Eines

Repositori que conté material per l'assignatura del grau de matemàtiques "Eines Informàtiques per les Matemàtiques".


## Notes

- Els únics fitxers que s'han d'editar són a `src/`
- Dins un bloc de codi es pot inserir `# begin hide` i `# end hide`. El text
  que hi hagi entre aquestes dues línies (incloses) es canviarà per `# SOLUCIÓ`.

## Passos a seguir

1. `mkdir -p output`

2. `jupytext --to ipynb src/$file.md -o output/$file.ipynb`

3. `cd src; ../process_file $file.ipynb ../output`

4. `curl -X PUT -u username:password -T ../output/$outfile https://mat.uab.cat/nextcloud/remote.php/dav/files/username/EIM/exercicis/`
