# zmk-config — Sofle (Fernando)

Config ZMK do teclado **Sofle** (split, 2x nice!nano v2, 2 OLEDs).
Doc completa no Obsidian: `07-Pessoal/Teclados/CONTEXTO-TECLADOS-ZMK.md`.

## ⚠️ Versão pinada em v0.3.0 (não voltar pra `main` sem checar)
ZMK `main` foi pro Zephyr 4.1 / LVGL 9 e **quebra o `zmk-nice-oled`**. Por isso está tudo pinado:
- `config/west.yml` → ZMK `revision: v0.3.0`
- `.github/workflows/build.yml` → `build-user-config.yml@v0.3.0`
- `build.yaml` → board `nice_nano_v2`

Voltar pro `main` (+ board `nice_nano//zmk`) só quando o nice_oled suportar LVGL 9.

## Build & flash
```bash
git push origin master                 # dispara GitHub Actions (~3-4 min)
gh run watch <id> --exit-status
gh run download <id>                    # baixa artefato "firmware"
# flash: double-tap reset -> monta /Volumes/NICENANO -> arrasta o .uf2
cp firmware.uf2 /Volumes/NICENANO/      # erro de I/O no fim = normal (reinicia)
```
- **Esquerda** (central) = `sofle_left_studio.uf2` (Studio + Bongo Cat)
- **Direita** (periférica) = `sofle_right...uf2` (Pokémon)
- `settings_reset...uf2` = limpa NVS (destrava OLED preso; perde pareamentos BT)

## firmware/ (pré-buildados, reuso rápido)
- `0-RESET-ambos-os-lados.uf2`
- `1-ESQUERDA-studio-gatinho.uf2`
- `2-DIREITA-pokemon.uf2`

## Teclas de tela (camada ADJUST = LOWER+RAISE)
- `` ` `` (crase) → desliga OLED (`&ext_power EP_OFF`)
- `TAB` → liga (`&ext_power EP_ON`) — **depois dar 1 reset** p/ a tela reacender (bug de re-init do ZMK)

## Animações (nice_oled) — `config/sofle.conf`
Opções são bools independentes: pra trocar, **desliga a atual (`=n`) e liga a nova (`=y`)**.
- Direita: `..._ANIMATION_PERIPHERAL_` POKEMON(atual) / CAT(Nyan) / GEM / HEAD / SPACEMAN / SMART_BATTERY
- Esquerda (WPM): `..._WPM_` BONGO_CAT(atual) / LUNA / SPEEDOMETER / GRAPH / NUMBER

## Recuperação ("não detecta")
1. **Tirar a bateria** (JST) — causa nº1.
2. USB + double-tap limpo (tirar a mão, esperar `NICENANO`).
3. nice!nano é imbrickável (bootloader UF2 protegido).
