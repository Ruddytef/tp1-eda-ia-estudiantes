# Impacto de la IA en estudiantes universitarios: análisis exploratorio de datos

Trabajo Práctico N° 1. Autor: Ruddy Toala.

## Objetivo

Analizar cómo el uso de herramientas de inteligencia artificial generativa se relaciona con el rendimiento académico, la retención de conocimiento y el bienestar de estudiantes universitarios. Para guiar el análisis se plantearon cinco hipótesis:

1. **H1.** A más horas semanales de IA, mayor riesgo de burnout.
2. **H2.** Las horas de estudio tradicional explican la mejora del GPA mejor que las horas de IA.
3. **H3.** Usar la IA para pedir respuestas directas se asocia a menor retención de conocimiento.
4. **H4.** A mejor habilidad con prompts, mayor retención: importa cómo se usa la IA, no cuánto.
5. **H5.** Prohibir la IA en la universidad no reduce su uso.

## Contexto del dataset

- **Fuente:** [Impact of AI on Students (Kaggle)](https://www.kaggle.com/datasets/laveshjadon/ai-impact-on-students)
- **Tamaño:** 50.000 filas (una por estudiante) y 16 columnas.
- **Contenido:** perfil del estudiante, hábitos de uso de IA, política de su universidad y resultados al final del semestre (notas, retención, ansiedad y riesgo de burnout).
- **Tipos de datos:** numéricos continuos, numéricos discretos, categóricos, ordinales y booleanos.

El dataset no tiene valores nulos ni filas duplicadas.

## Diccionario de datos

| Columna | Tipo | Descripción |
|---|---|---|
| `Student_ID` | Entero | Identificador único del estudiante |
| `Major_Category` | Categórica | Área de estudio: Humanities, Medical, Business, STEM, Arts |
| `Year_of_Study` | Ordinal | Año de carrera: Freshman, Sophomore, Junior, Senior, Graduate |
| `Pre_Semester_GPA` | Decimal | Promedio de notas antes del semestre (escala 0 a 4) |
| `Weekly_GenAI_Hours` | Decimal | Horas semanales de uso de IA generativa (0 a 40) |
| `Primary_Use_Case` | Categórica | Uso principal de la IA: Drafting, Ideation, Summarizing, Debugging, Direct_Answers |
| `Prompt_Engineering_Skill` | Ordinal | Habilidad para escribir prompts: Beginner, Intermediate, Advanced |
| `Tool_Diversity` | Entero | Cantidad de herramientas de IA distintas que usa (1 a 5) |
| `Paid_Subscription` | Booleana | Si paga una suscripción de IA (True / False) |
| `Traditional_Study_Hours` | Decimal | Horas semanales de estudio sin IA |
| `Perceived_AI_Dependency` | Entero | Cuánto siente que depende de la IA (escala 1 a 10) |
| `Institutional_Policy` | Categórica | Política de la universidad: Strict_Ban, Allowed_With_Citation, Actively_Encouraged |
| `Anxiety_Level_During_Exams` | Entero | Nivel de ansiedad en exámenes (escala 1 a 10) |
| `Post_Semester_GPA` | Decimal | Promedio de notas después del semestre (escala 0 a 4) |
| `Skill_Retention_Score` | Decimal | Puntaje de retención de conocimiento (0 a 100) |
| `Burnout_Risk_Level` | Ordinal | Riesgo de agotamiento: Low, Medium, High |

**Columnas creadas durante el análisis:**

| Columna | Descripción |
|---|---|
| `GPA_Change` | Diferencia entre el GPA después y antes del semestre. Positivo = mejoró |
| `AI_Usage_Level` | Horas de IA agrupadas en tres niveles: Bajo (0-5 h), Medio (5-15 h), Alto (15-40 h) |

## Metodología

1. **Lectura e integridad.** Revisión de la estructura con `head`, `tail`, `sample`, `info` y `describe`. Se verificó que no hubiera nulos ni estudiantes duplicados, y se revisaron los valores de cada columna categórica.
2. **Limpieza.**
   - Se estandarizaron los nombres de `Primary_Use_Case`, que mezclaban `/` y `_`.
   - Se definió el orden de las variables ordinales (año de estudio, habilidad con prompts y burnout) para que no aparezcan en orden alfabético en los gráficos.
   - Se creó la columna `GPA_Change` para medir la mejora de cada estudiante.
   - Se detectaron 2.583 outliers en horas de IA (más de 25.7 h por semana) con el método IQR. No se eliminaron porque son valores posibles y representan a los usuarios intensivos, que son justamente el grupo a estudiar en la H1.
3. **Análisis exploratorio.** Histogramas con KDE, boxplots, scatterplots, gráficos de barras, gráfico de torta y matriz de correlación.
4. **Prueba de normalidad.** Kolmogorov-Smirnov sobre las horas de estudio y de IA, interpretada junto a los histogramas.
5. **Conclusiones.** Respuesta a cada hipótesis con los datos obtenidos.

**Herramientas:** Python, pandas, numpy, matplotlib, seaborn y scipy.

## Conclusiones y hallazgos relevantes

Las cinco hipótesis se cumplieron, aunque no todas con la misma fuerza.

- **H1. Más horas de IA, más burnout.** Es el resultado más fuerte. Entre los estudiantes que usan poca IA, solo el 10% tiene riesgo alto de burnout; entre los que la usan más de 15 horas por semana, sube al 65%. Además, quienes más la usan se sienten más dependientes de ella y más ansiosos en los exámenes.
- **H2. Estudiar mejora más el GPA que usar IA.** Las horas de estudio tienen una relación moderada con la mejora del GPA (0.38); las horas de IA prácticamente no tienen relación (-0.05).
- **H3. Pedir respuestas directas reduce la retención.** Se cumple, pero la diferencia es pequeña: 73.7 frente a 78.1 de quienes usan la IA para depurar.
- **H4. Importa cómo se usa la IA, no cuánto.** Los estudiantes avanzados en prompts retienen 11 puntos más que los principiantes, aunque todos usan la IA más o menos las mismas horas.
- **H5. Prohibir la IA no reduce su uso.** Los estudiantes la usan casi lo mismo en universidades que la prohíben y en las que la fomentan, alrededor de 6 horas por semana.

**Conclusión final:** la IA no es buena ni mala en sí misma; lo que cambia los resultados es la forma de usarla. Cuando se usa para entender y resolver problemas, los estudiantes retienen más. Cuando se usa en exceso o para saltarse el esfuerzo, aparece más agotamiento y las notas no mejoran. Como prohibirla tampoco funciona, parece más útil enseñar a usarla bien.

**Limitaciones:** los resultados muestran relaciones entre variables, pero no prueban que una cause la otra. Por ejemplo, puede que la IA no cause el burnout, sino que los estudiantes ya agotados la usen más. La dependencia y la ansiedad son valores que cada estudiante se asignó a sí mismo, y algunas columnas tienen límites fijos (40 h de IA, 1 h de estudio, GPA máximo de 4.0) que pueden afectar algunas comparaciones.

## Estructura del repositorio

```
├── README.md
├── ai_student_impact_dataset_1.csv
└── tp1_eda_ia_estudiantes.ipynb
```

Para ejecutar el notebook se necesitan las librerías `pandas`, `numpy`, `matplotlib`, `seaborn` y `scipy`.
