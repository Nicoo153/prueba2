# Clínica Vitalis — EP2 | Agente funcional con RAG y memoria

**Asignatura:** Ingeniería de Soluciones con IA (ISY0101)  
**Caso:** clínica ficticia Vitalis, simulación académica. **No es un servicio médico.**

## Descripción
Continuación de EP1 (asistente RAG) hacia un agente con herramientas de consulta documental, consulta de cupos, registro de solicitudes ficticias, recuperación semántica de preferencias y continuidad conversacional. Usa **Google Colab**, LangChain/LangGraph, **Gemini 3.6 Flash**, ChromaDB, embeddings `all-MiniLM-L6-v2` y SQLite.

## Estructura
- `Vitalis_EP2_Google_Colab.ipynb`: notebook principal; ejecutar las celdas **en orden**.
- `diagramas/Arquitectura_Vitalis.png`: diagrama de alto nivel del sistema.
- `Informe_EP2_Clinica_Vitalis.pdf`: borrador técnico para completar con argumentos y conclusiones propias.
- `evidencias/`: depositar capturas de resultados de Colab sin claves ni datos reales.

## Ejecución reproducible
1. Abrir https://colab.research.google.com/ y cargar `Vitalis_EP2_Google_Colab.ipynb`.
2. En **Secretos** de Colab, guardar `GOOGLE_API_KEY` y permitir el acceso al notebook. No pegar la clave en código ni en este repositorio.
3. Ejecutar la instalación. Las advertencias de dependencias no equivalen siempre a fallo; si ocurre un error real, corregirlo antes de avanzar.
4. Ejecutar la configuración de directorios, RAG/Chroma, SQLite, memoria y pruebas deterministas.
5. En la celda del agente, verificar que el modelo sea `gemini-3.6-flash`. Debe estar habilitado en el proyecto de Google AI Studio.
6. Ejecutar secuencialmente pruebas A–F y revisar que Gemini invoque realmente herramientas (`ToolMessage`).
7. Para validar escritura, habilitar **expresamente** `EJECUTAR_PRUEBA_ESCRITURA=True` en la última celda adicional. **Modifica la base de prueba**; evitar repetirla sobre el mismo cupo.

## Herramientas
| Herramienta | Función |
|---|---|
| `buscar_documentos` | Recupera fragmentos del manual simulado desde ChromaDB. |
| `consultar_cupos` | Consulta la agenda de pruebas en SQLite. |
| `registrar_solicitud` | Inserta solicitud ficticia solo si `confirmacion=True`; ocupa el cupo. |
| `ver_solicitudes` | Verifica los registros ficticios. |
| `guardar_preferencia_simulada` | Escribe una preferencia administrativa ficticia en memoria vectorial. |
| `recuperar_preferencias` | Recupera preferencias relacionadas filtradas por sesión. |

## Memoria y persistencia
- **Corto plazo:** `InMemorySaver` de LangGraph mantiene mensajes por `thread_id` **solo mientras dure el proceso**.
- **Semántica:** ChromaDB conserva el índice documental y recuerdos por carpeta; filtra recuerdos según la sesión.
- **Agenda:** SQLite almacena cupos y solicitudes simuladas.
- `USAR_DRIVE=False` de forma predeterminada: la información en `/content` puede perderse al finalizar la sesión. Para conservación entre sesiones, usar `USAR_DRIVE=True` y permitir montaje de Drive.

## Evidencia realmente disponible
Las salidas compartidas en Colab muestran: consulta RAG de horario de Providencia y uso de `buscar_documentos`; búsqueda de cupos para perfil lipídico; memoria de preferencia de Maipú recuperada; alternativas ante las 11:00 ocupadas; rechazo de consejo farmacológico. **La escritura confirmada aún requiere ejecución y captura de evidencia.**

## Limitaciones
- Datos de clínica, cupos y pacientes **ficticios**; no hay conexión con sistemas reales.
- Las respuestas del LLM pueden añadir información no documentada; contrastar con el texto fuente.
- La memoria corta `InMemorySaver` no sobrevive reinicios; la semántica persiste solo si el directorio se conserva.
- La confirmación de escritura se valida por un booleano en la herramienta; en un sistema productivo se requerirían autenticación, autorización y confirmación estructurada fuera del LLM.
- No almacenar información sensible o datos clínicos reales.

## Checklist de evidencias y entrega
- [ ] Capturas visibles de ejecución A–F y de ToolMessages (ocultar IDs sensibles y claves).
- [ ] Captura de inserción SQLite con confirmación explícita, verificada por `ver_solicitudes`.
- [ ] Probar memoria entre dos turnos; documentar que se pierde al reiniciar si no hay persistencia.
- [ ] Revisar el informe (máximo 5 páginas) y añadir argumentos técnicos, conclusión y reflexión **escritos por el estudiante**.
- [ ] Publicar repositorio en GitHub/GitLab; pegar URL de repositorio en AVA y correo docente, según indicaciones.

## Uso ético de IA
Se debe declarar toda herramienta generativa utilizada para diagramas y edición. La evaluación exige conclusiones, justificaciones y reflexiones de autoría propia; los documentos suministrados incluyen espacios para completarlas.

## Referencias para consultar
- LangChain: https://docs.langchain.com/
- LangGraph: https://docs.langchain.com/oss/python/langgraph/overview
- Chroma: https://docs.trychroma.com/
- Google Gemini API: https://ai.google.dev/gemini-api/docs
- Sentence Transformers: https://www.sbert.net/
- Duoc UC — IA: https://bibliotecas.duoc.cl/ia
