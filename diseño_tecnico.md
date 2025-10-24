# ⚙️ Diseño Técnico Detallado de AetherOS

Este documento detalla los aspectos técnicos y las justificaciones de diseño de **AetherOS**, siguiendo los requerimientos del curso Sistemas Operativos I.

## 1. Nombre del Sistema Operativo y Propósito

* **Nombre:** **AetherOS** ✨
* **Propósito:** Sistema operativo de propósito especial para la **investigación científica colaborativa y el manejo de Big Data experimental** con un enfoque en la **computación cuántica simulada** y el análisis de conjuntos de datos masivos.

## 2. Tipo de Núcleo Elegido y Justificación

* **Tipo de Núcleo:** **Núcleo Híbrido Modificado**.
* **Justificación:**
    * Combina la **estabilidad y seguridad** del **Microkernel** (controladores y sistemas de archivos primarios en espacio de usuario) con la **velocidad** del **Monolítico** para tareas críticas.
    * **Modificación:** Los servicios de alto rendimiento (e.g., el *Quantum Simulation Engine* o *QSE* y el gestor de *Big Data I/O*) se cargan como **módulos dinámicos en espacio de núcleo** solo cuando son necesarios y tras una verificación de integridad, optimizando el rendimiento para cargas científicas masivas sin comprometer la modularidad.

## 3. Gestión de Procesos

* **Planificación (HPTL):** **Planificador Jerárquico por Prioridad y Tipo de Carga**. Prioriza tres niveles: 1) Seguridad/Tiempo Real, 2) CIENTÍFICO CRÍTICO (usando *Fair-Share Scheduling* para equidad entre proyectos), y 3) General.
* **Estados:** Se añaden los estados clásicos:
    * **Quantum-Wait:** Espera la resolución de una subrutina ejecutada en el QSE.
    * **Data-Stream:** Proceso activamente transmitiendo *Big Data* que no debe ser interrumpido (para evitar corrupción).
* **Concurrencia:** Utiliza **Semáforos Cuánticos (QSema)**. Estos permiten una gestión más predictiva de la contención de recursos al ofrecer información probabilística sobre cuándo se liberará un recurso.

## 4. Gestión de Memoria

* **Esquema Principal:** **Paginación Multidireccional con Segmentación Lógica**.
    * La **Segmentación** organiza el espacio de direcciones por la naturaleza del dato (código, datos experimentales, pila), proveyendo protección lógica.
    * La **Paginación** maneja el espacio de direcciones virtual. Las páginas tienen un **tamaño dinámico** (hasta 1GB) para acomodar los bloques de datos científicos masivos.
* **Asignación (*Swapping*):** Implementa el algoritmo **LRU-Predictivo (LRUP)**. Predice qué páginas necesitará el proceso de análisis de datos a continuación (basado en el patrón de acceso histórico) para precargar o mantenerlas, reduciendo latencia en operaciones de I/O masivas.

## 5. Sistema de Archivos

* **Nombre:** **AetherFS (Aether File System)**.
* **Estructura:** **Sistema de Archivos Distribuido por Contenido (CDFS)**. Los datos son almacenados en un clúster y son indexados por **metadatos semánticos** (no por una ruta de directorios tradicional), lo que facilita consultas complejas sobre la naturaleza del dato.
* **Jerarquía:** Virtual, basada en la consulta de metadatos. La estructura física es plana y distribuida.
* **Permisos:** **Control de Acceso Basado en Atributos (ABAC)**. Los permisos se otorgan dinámicamente según la combinación de atributos del usuario (p. ej., 'Científico' + 'Miembro del Proyecto ATLAS' + 'Conectado desde Red Segura').

## 6. Mecanismos de Seguridad

* **Autenticación:** **Biométrica Multinivel (BMA)**, combinando conocimiento (contraseña) e inherencia (escaneo de dispositivo/biometría), con claves almacenadas en un *Hardware Security Module (HSM)*.
* **Control de Acceso:** **ABAC** y **Aislamiento a Nivel de Núcleo**. Los procesos críticos se ejecutan en *sandboxes* estrictos con aislamiento de memoria forzado por *hardware* para evitar fugas de información o interferencia entre experimentos.
* **Cifrado:** **Cifrado Post-Cuántico (PQC)**. Utiliza algoritmos robustos (e.g., Kyber, Dilithium) para proteger los datos en reposo y en tránsito contra posibles ataques de computadoras cuánticas futuras, garantizando la longevidad de los datos científicos.

## 7. Interfaz de Usuario

* **GUI Adaptativa y Consola de Comando por Voz (VCC):**
    * **GUI:** Interfaz **holográfica/3D** optimizada para la visualización de datos complejos y simulaciones. Interacción mediante **gestos**.
    * **VCC:** Una CLI avanzada activada por **voz** para el envío de trabajos (*jobs*), la configuración del clúster y la gestión de datos, permitiendo a los científicos gestionar el entorno sin desviar su atención de las visualizaciones complejas.

## 8. Comparación con un Sistema Operativo Real

| Característica | AetherOS (Ficticio) | Linux (Windows Server/Android) |
| :--- | :--- | :--- |
| **Propósito** | Cómputo científico, Big Data, simulación cuántica. | Propósito general (servidor, escritorio, móvil). |
| **Núcleo** | Híbrido Modificado | Monolítico/Híbrido (Windows)/Microkernel (Android) |
| **Almacenamiento** | AetherFS (Distribuido, Metadatos) | Ext4, NTFS (Jerárquico, basado en nombres) |
| **Seguridad** | Cifrado Post-Cuántico (PQC), ABAC | AES, DAC/RBAC (Permisos de usuario/grupo) |

**Conclusión de la Comparación:** AetherOS se diferencia en su **especialización extrema**. Sacrifica la flexibilidad de sistemas como Linux/Windows para obtener un **rendimiento superior y una seguridad futurista** en el nicho de la ciencia de datos masivos. Su enfoque en el **ABAC** lo hace ideal para entornos de colaboración de alta seguridad.
