# Audios de la voz de Umbral

Aquí van los audios de la voz del entrenador, una carpeta por idioma:

```
public/audio/voz/
  es/      español (voz "Martin")
  en/      inglés de EE. UU. (voz "Philip")
  pt-BR/   portugués de Brasil (voz "Túlio")
```

## Formato

- **MP3 mono**, 44,1 kHz (o 22,05 kHz), unos 64 kbps. Ligero: cada clip pesa pocos KB.
- **Nombre del archivo = id del clip** del guion, más `.mp3`. Ejemplos: `rep_05.mp3`, `fase_sosten.mp3`, `tope_rango_2.mp3`.
- Sin silencio al principio ni al final (recortar a menos de 50 ms): el conteo depende de que suene a tiempo.
- Volumen parecido en todos los clips (normalizar a unos −16 LUFS).

## De dónde salen

- **Texto y tono:** `src/content/<idioma>/voice.json` → `clips.<id>.texto` y `clips.<id>.direccion`.
  Tabla para revisar los tres idiomas juntos: `docs/voz/guion.md`.
- **Voz:** ElevenLabs, con la cuenta de Edwin y la voz elegida para cada idioma. Las voces se usan con licencia
  comercial según su plan (confirmarlo en las condiciones de ElevenLabs antes de publicar).

## Cómo generarlos (automático)

Con el programa `scripts/generar-voz.mjs`, en la computadora de Edwin (la nube de Claude no puede descargar de ElevenLabs):

1. Copia tu clave de ElevenLabs (en elevenlabs.io → tu perfil → API Keys). No la pegues en ningún archivo del proyecto.
2. En una terminal, dentro de la carpeta del proyecto, ponla en la variable de entorno:
   - Windows (PowerShell): `$env:ELEVENLABS_API_KEY="tu_clave"`
   - Mac o Linux: `export ELEVENLABS_API_KEY="tu_clave"`
3. `node scripts/generar-voz.mjs --estimar` → dice cuántos clips y caracteres faltan, sin gastar créditos.
4. `node scripts/generar-voz.mjs --idioma es --prueba` → genera 3 clips para escucharlos antes de hacerlo todo.
5. `node scripts/generar-voz.mjs` → genera todos los que falten (los que ya existen no se repiten).
   Para rehacer un idioma completo: `--idioma en --rehacer`.

Voces: español "Martin", inglés "Philip", portugués "Túlio" (ids dentro del programa). Modelo `eleven_v4`, que usa la `direccion` del guion como etiqueta de tono.

## Cómo regenerar un clip a mano

1. Cambia el texto en `voice.json` del idioma (y la fila de `docs/voz/guion.md`).
2. Genera en ElevenLabs solo los clips que cambiaron, con la misma voz y ajustes, usando la `direccion` como guía de tono.
3. Exporta en MP3 mono y guárdalo aquí con el nombre exacto del id (`<id>.mp3`), sustituyendo el anterior.
4. Comprueba en la app: Ajustes → Voz → "Probar la voz", y una serie corta con la voz encendida.

Si falta un archivo, la app lee el texto con la voz del teléfono, así que nunca se queda muda.
Si se **añade o cambia un id**, hay que tocar también `src/domain/voice/clips.ts`; la prueba `src/features/voice/voice.test.ts` avisa si falta algo.
