# GeoRA — Geometría espacial en Realidad Aumentada

Prototipo funcional para el proyecto *Aplicación de realidad aumentada para el aprendizaje
de cuerpos geométricos y desarrollos planos* (Matemática Educativa y Computación, UNAN-León).

## Qué hace

- Muestra 5 cuerpos sobre un marcador impreso: **cubo, prisma rectangular, pirámide cuadrangular, cilindro y cono**.
- El control **Desarrollo** despliega el cuerpo en su **red plana** y lo vuelve a armar, con animación continua.
- Las **medidas son editables** (sliders) y el panel recalcula en vivo el **área lateral, el área total y el volumen**,
  mostrando la fórmula con los valores sustituidos, no solo el resultado.
- Cuenta **caras, vértices y aristas** y comprueba la **fórmula de Euler** (C + V − A = 2) en los poliedros,
  advirtiendo que no aplica en cilindro y cono.
- Funciona **sin internet** una vez cargada la primera vez (PWA con service worker) y **sin cámara**
  en modo escritorio, para computadora.

## Cómo ejecutarlo

La cámara solo funciona en `https://` o en `localhost`. Hay dos caminos:

**A. Prueba local (computadora)**

```bash
cd geora
python3 -m http.server 8000
```
Abrir `http://localhost:8000/index.html`.

**B. Publicar para usarlo en los celulares del aula (recomendado)**

1. Crear un repositorio en GitHub y subir toda esta carpeta.
2. Settings → Pages → Source: `main` / carpeta raíz → Save.
3. En pocos minutos queda en `https://USUARIO.github.io/REPO/`.
4. Abrir esa dirección en el celular, pulsar **Activar RA** y aceptar el permiso de cámara.
5. Una vez abierta, la aplicación queda cacheada y sigue funcionando sin datos.

## El marcador

Abrir `marcador.html` e imprimir **al 100 %** en papel blanco mate. El cuadro negro debe medir
unos 11 cm de lado. Es el marcador *Hiro* de ARToolKit.

Recomendaciones de campo: luz pareja, sin brillos ni sombras duras sobre el papel, hoja plana
(no doblada), cámara a 30–50 cm.

## Estructura

```
index.html            aplicación completa (interfaz + motor geométrico + RA)
marcador.html         marcador imprimible (imagen incrustada, no requiere internet)
manifest.json         metadatos PWA
sw.js                 service worker (uso sin conexión)
vendor/three.global.js   Three.js r164 compilado como global
vendor/ar-threex.js      AR.js 3.4.8 (seguimiento de marcadores)
data/patt.hiro           patrón del marcador
data/camera_para.dat     parámetros de calibración de cámara
```

## Cómo está construido (para el capítulo de metodología)

El núcleo no dibuja cuerpos 3D prefabricados. Cada cuerpo se define como una **red plana**
(un conjunto de polígonos 2D) más un **árbol de bisagras**: qué cara se pliega sobre cuál,
sobre qué arista y con qué ángulo.

- El ángulo de pliegue de cada bisagra es `π − ángulo diedro` del cuerpo armado.
- Armar el cuerpo = girar todas las bisagras a su ángulo; desplegarlo = llevarlas a cero.
- El parámetro `t ∈ [0,1]` interpola ambos estados, y de ahí sale la animación.

Consecuencia didáctica: el cuerpo y su desarrollo plano **no son dos modelos distintos**,
son el mismo objeto en dos estados. Eso es exactamente lo que se quiere que el estudiante
comprenda, y es también el aporte técnico defendible del prototipo.

Las superficies curvas (cilindro y cono) se aproximan con 40 caras planas, pero **el área y el
volumen se calculan con las fórmulas exactas** (π), no con la aproximación poligonal.

## Límites conocidos del prototipo

- Un solo marcador: el cuerpo se elige desde la interfaz, no por marcadores distintos.
- El seguimiento por marcador es sensible a la iluminación; conviene probar en el aula real.
- No guarda progreso del estudiante ni registra respuestas (sería la siguiente iteración).
- Requiere navegador con WebGL: probar antes qué teléfonos tienen los alumnos.

## Créditos

Three.js (MIT) · AR.js / ARToolKit (LGPL v3) · Marcador Hiro de ARToolKit.
