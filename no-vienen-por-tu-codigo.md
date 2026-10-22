# No vienen por tu código

### Por qué los grandes ataques de 2025–2026 golpean todo lo masivo —npm, el AUR, la Snap Store, GitHub, WordPress— y qué dice eso sobre dónde debe vivir la verdad de tu software

*Por Juan Carlos Isaza — abogado y programador. Xiliux.*

---

Hay una pregunta que desarma casi todo el miedo mal planteado sobre la seguridad del software libre: **¿por qué alguien hackearía GitHub solo para obtener código?**

El código abierto ya es abierto. Cualquiera puede clonarlo esta misma tarde, leerlo entero, estudiarlo. No hay que forzar ninguna puerta para *leer* lo que, por definición, está publicado. Si el premio fuera el código, no habría ataque: habría un `git clone`.

El premio es otro, y entenderlo bien cambia toda la conversación.

## El premio no es leer tu código. Es que el mundo ejecute el suyo con tu nombre encima.

La seguridad informática clásica se preocupa por la **confidencialidad**: que nadie lea lo que no debe. Pero los ataques que de verdad hacen daño a escala en estos dos años no van contra la confidencialidad. Van contra la **integridad**: no quieren ver tu código, quieren *cambiarlo* —o cambiar algo de lo que tú dependes— para que millones de máquinas ejecuten su carga sin sospechar nada, amparadas en la confianza que tú ya te ganaste.

El objetivo nunca es la víctima final. Es el **punto de confianza concentrada**: el único nodo del que cuelgan miles o millones de proyectos. Y la propagación no la hace el atacante —la hace la propia infraestructura de distribución: el gestor de paquetes, la actualización automática, la dependencia transitiva—. El atacante no persigue a nadie. Las víctimas vienen solas a actualizarse.

## El mismo ataque, seis capas distintas

Cuando uno pone los incidentes grandes de 2025–2026 lado a lado, deja de ver seis ataques y empieza a ver **uno solo**, aplicado a seis capas de la misma cadena de confianza:

- **npm.** Las campañas del gusano *Shai-Hulud* y su variante *CHAINDROP* comprometieron, según quién cuente, entre 400 y más de 2.200 paquetes y artefactos a lo largo de 2026. Es un gusano **autopropagante**: con las credenciales robadas de un mantenedor, introduce puertas traseras automáticamente en todos los demás paquetes que esa persona puede publicar, y hasta busca su servidor de mando en un contrato inteligente de Ethereum. ([Elastic Security Labs](https://www.elastic.co/security-labs/shai-hulud-chaindrop-npm-supply-chain), [Unit 42](https://unit42.paloaltonetworks.com/monitoring-npm-supply-chain-attacks/))

- **AUR (Arch Linux).** En julio de 2025 se subieron paquetes como `librewolf-fix-bin` que instalaban el troyano *Chaos RAT*: el `PKGBUILD` clonaba un repositorio del atacante y lo ejecutaba durante la compilación, porque `makepkg` corre **sin aislamiento**. ([BleepingComputer](https://www.bleepingcomputer.com/news/security/arch-linux-pulls-aur-packages-that-installed-chaos-rat-malware/))

- **Snap Store (Canonical).** Conviene ser precisos: no hubo una brecha en la infraestructura de Canonical. Hubo **secuestro de cuentas de publicadores legítimos** —dominios caducados que los atacantes recompraron para quedarse con el correo y, con él, con la cuenta de Snapcraft— para empujar billeteras cripto falsas que roban la frase semilla. La confianza era la puerta. ([Help Net Security](https://www.helpnetsecurity.com/2026/01/21/linux-malware-snap-store/))

- **GitHub.** Cuentas de mantenedores tomadas, *commits* huérfanos que se saltan la revisión de código, flujos de integración continua abusados. En un caso, la cuenta comprometida de un empleado de una gran distribución sirvió para empujar código sin pasar por ninguna revisión.

- **WordPress y PHP.** Alrededor del **70 % de los sitios web cuyo lenguaje de servidor se conoce corre PHP** ([W3Techs](https://w3techs.com/technologies/details/pl-php)), y WordPress por sí solo es cerca del **43 % de toda la web**. Su superficie real de ataque no es el núcleo —razonablemente auditado— sino el ecosistema de **plugins y temas de terceros**: decenas de miles, de calidad desigual, muchos abandonados, todos corriendo con los permisos del núcleo. El mismo patrón del AUR y de npm, pero a escala de medio internet.

- **La inteligencia artificial, que no es una capa más sino el acelerante de todas.** En noviembre de 2025 se documentó la primera campaña de ciberespionaje conducida en su mayor parte por una IA: un grupo estatal automatizó entre el **80 % y el 90 %** de las operaciones tácticas —reconocimiento, explotación, robo de credenciales, exfiltración— contra unas treinta organizaciones, evadiendo las salvaguardas del modelo al disfrazar cada paso como una prueba de seguridad rutinaria. ([The Hacker News](https://thehackernews.com/2025/11/chinese-hackers-use-anthropics-ai-to.html))

## No es un ejército con un general. Es un enjambre con un incentivo.

Es tentador concluir que detrás de todo esto hay una red única y coordinada. La evidencia dice algo más incómodo —y más difícil de combatir—. Los propios investigadores que rastrean estas campañas dejan la atribución abierta y advierten que la **actividad imitadora** hace cada vez más difícil atribuir las oleadas a un mismo grupo.

O sea: no hace falta un titiritero. Lo que hay es un **mercado con un incentivo compartido**. El vector de la cadena de suministro funciona tan bien —un compromiso, miles de víctimas aguas abajo— que todos los actores migran hacia él e imitan al que tuvo éxito. La IA acelera esa imitación hasta volverla casi instantánea. El resultado se *ve* coordinado, pero el mecanismo no es conspiración: es **selección**. El vector que mejor rinde es el que todos copian.

Esto no debería tranquilizar a nadie. Un enjambre sin centro no se puede decapitar —no hay líder que arrestar—, se replica más rápido que cualquier coordinación, y el incentivo que lo mueve no desaparece parcheando a un actor.

## La ley de fondo: la escala es la vulnerabilidad

Si hay que resumirlo en una frase: **lo que hace valioso a un componente —que millones confíen en él— es exactamente lo que lo convierte en blanco.** Popularidad y seguridad están en tensión directa. Cada nuevo proyecto que depende de ti aumenta tu valor como objetivo. Por eso se ataca "todo lo masivo": no porque esté peor construido, sino porque concentra más confianza por unidad de esfuerzo del atacante.

Y aquí entra una dimensión que, como abogado, no puedo pasar por alto. La integridad de la cadena de suministro está dejando de ser un problema puramente de ingeniería para convertirse en una **buena práctica exigible**. Los marcos de protección de datos —la Ley 1581 de 2003 en Colombia, y sus equivalentes en el resto del mundo— responsabilizan a quien trata datos personales por su integridad y seguridad, y empujan en esa dirección: verificar aquello de lo que dependes se vuelve parte de la diligencia esperable. "Confié en el paquete" es, cada vez, una defensa más débil; elegir en qué se confía empieza a contar.

## Lo que yo hice, con su precio

Hace un tiempo tomé una decisión incómoda para mi propio trabajo: **el origen de la verdad de mi código dejó de estar en una plataforma pública.** Vive en espejos locales, y las entregas viajan firmadas y verificables, sin depender de que un registro remoto me sirva el archivo correcto.

No lo cuento como receta universal ni como alarde. Lo cuento con su costo, que es real: perdí descubribilidad, perdí el respaldo externo gratuito que da una plataforma grande, y perdí oportunidades de financiación que exigen exponer el código en abierto. Lo haría igual, porque trabajo en seguridad y en criptografía, y para un proyecto así que te extraigan o te alteren el código no es un incidente: es un argumento en contra del producto. Pero quien considere este camino merece ver la factura completa, no una postal.

## Qué puede hacer cualquiera

Si el ataque es contra la **confianza concentrada y auto-distribuida**, la defensa no es levantar un muro más alto. Es **reducir y verificar la confianza**:

1. **Origen verificable por ti mismo.** Firmas reproducibles, sumas que puedes comprobar. No "confía en que el registro te sirvió el archivo bueno": compruébalo.
2. **No dependas de un único distribuidor.** El punto que, al caer, cae para todos, es el punto que no deberías tener.
3. **Minimiza la superficie de terceros.** Cada dependencia es un nodo de confianza que no controlas. La dependencia más segura es la que no agregaste.

Nada de esto es gratis ni cómodo. Pero es coherente con lo que la evidencia de estos dos años nos está gritando: la masividad concentra confianza, la confianza concentrada es el blanco, y la única salida de fondo es volver esa confianza **verificable y distribuida** otra vez.

No vienen por tu código. Vienen por la confianza que otros depositan en él. Y esa confianza, hoy, hay que poder demostrarla.

---

*Juan Carlos Isaza es abogado y programador. Construye [Quipu](https://xiliux.com), una biblioteca de cifrado y codificación post-cuántica verificable, con una cadena de suministro reproducible y sin red. Escribe sobre seguridad del software bajo la marca Xiliux.*
