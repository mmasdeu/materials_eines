# Material per Eines

Repositori que conté material per l'assignatura del grau de matemàtiques "Eines Informàtiques per les Matemàtiques".


## Notes

- Els únics fitxers que s'han d'editar són a `src/`
- Dins un bloc de codi es pot inserir `# begin hide` i `# end hide`. El text
  que hi hagi entre aquestes dues línies (incloses) es canviarà per `# SOLUCIÓ`.

## Passos a seguir

1. `mkdir -p "nbexercises" "nbsolutions"`

2. `jupytext --to ipynb src/$file.md -o nbsolutions/$file.ipynb`
3. `./make-student-version nbsolutions/$file.ipynb > nbexercises/$file.ipynb`

