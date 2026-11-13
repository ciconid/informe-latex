# informe-latex

Skill personal de [Claude Code](https://claude.com/claude-code) para redactar informes y documentos en PDF con LaTeX: preámbulo propio (cajas, íconos TikZ, babel español), compilación con `latexmk` y revisión visual del PDF.

## Archivos

- `SKILL.md`: instrucciones de la skill (estructura del documento, reglas de redacción, citas, compilación).
- `preambulo.tex`: preámbulo común (paquetes, colores, cajas `recuadro`/`destacado`/`aviso`/`extras`, macros de citas, íconos TikZ).
- `ejemplo.tex`: documento mínimo que usa todo lo del preámbulo.

## Instalación en otra máquina

```bash
git clone https://github.com/ciconid/informe-latex.git ~/repos/informe-latex
mkdir -p ~/.claude/skills
ln -s ~/repos/informe-latex ~/.claude/skills/informe-latex
```

Para probar el ejemplo: `latexmk -pdf -interaction=nonstopmode ejemplo.tex` (requiere una distribución TeX con `latexmk`, `tcolorbox`, `tikz` y `babel-spanish`).
