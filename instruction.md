# Instrucciones para Codex - Página de repaso de Aprendizaje Reforzado

## Objetivo
Mantener y mejorar una página HTML de repaso integrada para la materia Aprendizaje Reforzado. La organización debe ser **por temas**, no por clase. Las clases solo son fuente del contenido.

El foco de la página es estudiar para parciales con preguntas de este estilo:
- explicar conceptos y algoritmos;
- comparar métodos;
- justificar qué algoritmo conviene en un escenario;
- derivar fórmulas típicas;
- indicar qué problema resuelve cada mejora y cómo cambia el algoritmo.

## Estructura del proyecto

```text
rl_repaso_site/
├── index.html                  # Página principal, standalone, con CSS y JS inline
├── instruction.md              # Este archivo
└── assets/
    ├── slides_manifest.json    # Metadata de capturas seleccionadas
    └── slides/                 # Capturas JPG extraídas de las diapositivas
```

No hay build step. Para ver la página:

```bash
cd rl_repaso_site
python -m http.server 8000
# abrir http://localhost:8000
```

También se puede abrir `index.html` directamente en el navegador.

## Principios de edición

1. **No reorganizar por clase.** Mantener secciones temáticas:
   - Fundamentos
   - Imitación
   - Policy Gradient
   - Actor-Crítico
   - Métodos de valor
   - DQN y mejoras
   - Gradientes de política avanzados
   - Actor-Crítico avanzado
   - Comparaciones rápidas
   - Preguntas tipo parcial

2. **Cada algoritmo debe tener:**
   - idea central;
   - fórmula o pseudocódigo mínimo;
   - qué problema resuelve;
   - limitaciones;
   - preguntas probables de parcial.

3. **Cada mejora debe escribirse como problema → cambio → efecto.** Ejemplos:
   - replay buffer → correlación temporal → muestreo aleatorio de transiciones;
   - target network → target móvil → red congelada/actualizada lento;
   - Double DQN → sobreestimación → separar selección y evaluación;
   - TD3 delayed updates → crítico ruidoso → actualizar actor menos seguido;
   - SAC entropía → exploración insuficiente → recompensa + αH.

4. **Preferir precisión conceptual sobre texto decorativo.** La página es para estudiar, no para vender el tema.

5. **Mantener fórmulas legibles sin dependencias externas.** Actualmente se usan bloques `.formula` en texto plano. Si se agrega MathJax/KaTeX, hacerlo opcional y dejar fallback claro.

## Capturas de diapositivas

Las capturas están en `assets/slides/`. La metadata está en `assets/slides_manifest.json` con:
- `id`
- `pdf`
- `page`
- `title`
- `desc`
- `file`

Para insertar una nueva captura en HTML, usar este patrón:

```html
<figure class="slide-card">
  <img src="assets/slides/nombre.jpg" alt="Descripción" loading="lazy">
  <figcaption><strong>Título</strong><span>Descripción breve.</span></figcaption>
</figure>
```

## Contenido crítico que no se debe perder

### Fundamentos
- MDP/POMDP, estado vs observación.
- Objetivo como esperanza sobre trayectorias.
- V, Q y A.
- Muestrear, modelar, mejorar.

### Imitación
- Behavior cloning como supervisado.
- Desplazamiento distribucional.
- DAgger.
- Multimodalidad.

### Policy Gradient
- Derivación con log-derivative trick.
- REINFORCE.
- Baselines y ventaja.
- Importance sampling.
- On-policy vs off-policy.

### Actor-Crítico
- Crítico como baseline.
- Bootstrap y TD error.
- Sesgo-varianza.
- n-step returns y GAE.
- Batch, online, sincrónico, asincrónico.

### Métodos de valor
- Policy Iteration.
- Value Iteration.
- Fitted Value Iteration.
- Fitted Q Iteration.
- Q-learning y TD error.
- Convergencia tabular vs no tabular.

### DQN y mejoras
- Replay buffer.
- Target network.
- Polyak averaging.
- Double DQN.
- Retornos a n-pasos.
- Acciones continuas y problema del argmax.
- DDPG.
- Rainbow: PER, Dueling, Distributional, Noisy, n-step, Double.
- R2D2, NGU, Agent57.

### Gradientes de política avanzados
- PG como Policy Iteration suave.
- Ratio π_new/π_old.
- Surrogate objective.
- KL vs norma de parámetros.
- Gradiente natural y Fisher.
- TRPO.
- PPO Penalty.
- PPO Clip.

### Actor-Crítico avanzado
- Limitaciones de DDPG.
- TD3: clipped double Q, delayed updates, target policy smoothing.
- SAC: entropía, α, soft Bellman backup, doble Q, versiones con/sin V.

## Mejoras recomendadas para próximas iteraciones

1. **Modo quiz:** convertir las preguntas tipo parcial en tarjetas con botón "mostrar respuesta" y puntaje.
2. **Más derivaciones paso a paso:** especialmente Policy Gradient, Bellman, GAE y PPO surrogate.
3. **Ejercicios resueltos:** agregar problemas cortos tipo parcial con solución compacta.
4. **Buscador mejorado:** resaltar coincidencias y ocultar secciones vacías.
5. **Modo impresión:** mejorar CSS `@media print` para generar un PDF de estudio.
6. **Separar archivos:** si el HTML crece mucho, mover CSS a `styles.css` y JS a `app.js`.
7. **Agregar glosario:** MDP, POMDP, TD error, bootstrap, off-policy, target network, KL, entropy, etc.
8. **Agregar mapa de decisión:** árbol interactivo para elegir algoritmo según modelo conocido/desconocido, acciones discretas/continuas y disponibilidad de datos.
9. **Agregar referencias internas:** links desde cada mejora a la sección donde se explica el algoritmo base.

## Criterio de calidad

Antes de cerrar una modificación, verificar:
- La página abre sin errores en navegador.
- Las imágenes siguen cargando desde rutas relativas.
- No se perdió la organización temática.
- Las comparaciones mantienen el formato problema → cambio → efecto.
- Las preguntas tipo parcial tienen respuestas justificadas.
- El contenido sigue siendo autocontenido: no depende de recordar qué decía una clase específica.
