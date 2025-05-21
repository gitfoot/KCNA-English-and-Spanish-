


---

### **Pods (Los Jugadores Individuales)**

Un **Pod** es como un **jugador individual** en el campo, junto con su equipamiento básico (balón, botas, uniforme).

* Cada Pod es una **unidad fundamental que ejecuta una parte de tu aplicación**, como un jugador realiza una acción específica en el juego.
* Si un jugador se lesiona y tiene que salir del campo (el Pod falla o se "crashea"), simplemente deja de jugar. No hay un mecanismo inherente para que un jugador de reemplazo aparezca mágicamente por sí mismo. Su vida útil es mientras esté en el campo o hasta que termine el partido.

---

### **ReplicaSets (El Entrenador Asistente / Supervisor de Plantilla)**

El **ReplicaSet** es como el **entrenador asistente o el supervisor de la plantilla** en un equipo de fútbol. Su **única obsesión** es asegurarse de que siempre haya un **número exacto de jugadores clave** en el campo o listos para entrar, según la estrategia del partido.

* Si un jugador titular se lesiona o es expulsado, el entrenador asistente **inmediatamente busca un sustituto** en la banca para mantener el número deseado de jugadores en el campo.
* Si por alguna razón hay más jugadores de los permitidos en el campo, el entrenador asistente se asegura de que algunos salgan para cumplir con las reglas.
* A él no le importa si el jugador es un novato o una estrella, solo que el número *X* de posiciones estén cubiertas por jugadores.


