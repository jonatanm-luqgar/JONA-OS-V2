# JONA OS · Paquete integrado

## Estructura
- `index.html`: portal principal y navegación integrada.
- `modules/pmi/`: JONA PMI, adaptado a autenticación anónima y espacio compartido.
- `modules/agenda.html`: Mi agenda personal, con clave de sincronización común `jona-os-shared`.
- `modules/rpg.html`: JonaRPG, conserva su documento compartido `rpg/shared`.
- `firestore.rules`: reglas para el esquema de acceso sin contraseña.

## Despliegue
1. Haz una copia de seguridad de tu proyecto Firebase y de las tres aplicaciones actuales.
2. En Firebase Console > Authentication > Sign-in method, activa **Anonymous**.
3. En Firestore > Rules, revisa y publica el contenido de `firestore.rules`. Estas reglas permiten a cualquier visitante que abra la aplicación autenticarse anónimamente y leer/escribir el espacio compartido. No son adecuadas para datos privados o sensibles.
4. Sube el contenido completo de esta carpeta a la raíz de un repositorio GitHub Pages (o al hosting que uses). No subas solo `index.html`: deben conservarse las rutas `modules/`.
5. Abre el enlace publicado. No se requiere registro ni contraseña.

## Importante sobre los datos existentes
- JonaRPG conserva la ruta `rpg/shared`; al abrir el módulo debería recibir los datos existentes si las reglas permiten su lectura.
- Mi agenda conserva la clave `agenda_sync_key` que ya exista en el navegador; si no hay una, usa `jona-os-shared`. Para sincronizar otros dispositivos, todos deben utilizar exactamente la misma clave. Los datos de una clave antigua distinta no se copian automáticamente.
- JONA PMI pasa a usar `users/shared`. Los datos que estaban bajo `users/{UID}` no se migran automáticamente. Si había información en esa cuenta, inicia sesión en la versión original y exporta/recupera los datos antes de cambiar de esquema.

## Limitación de seguridad del acceso libre
La autenticación anónima elimina la pantalla de usuario/contraseña, pero no verifica que quien abre el enlace seas tú. Cualquier persona con el enlace puede crear una sesión anónima y modificar los datos compartidos. Para uso personal real, lo recomendable es añadir en el futuro una llave de acceso o un proveedor de identidad, aunque no haya contraseña manual. La API key web de Firebase identifica el proyecto, no es una credencial administrativa; nunca publiques claves privadas de cuentas de servicio.

## Mejoras de arquitectura incluidas
- Portal unificado con navegación persistente y módulos aislados para evitar colisiones de CSS/JavaScript.
- Los tres módulos conservan sus interfaces y capacidades originales.
- PMI usa sesión anónima y colección compartida.
- Agenda usa una clave de sincronización común entre dispositivos.
- Reglas de Firestore explícitas para las rutas usadas por el paquete.

Este paquete integra las aplicaciones en una experiencia única, pero mantiene sus modelos de datos separados para reducir el riesgo de romper funcionalidades. La unificación completa de colecciones requeriría una migración de datos controlada y validada.
