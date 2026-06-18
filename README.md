2do Parcial - Parte Practica
Que se solicita:

El codigo tiene 10 errores. Recae en usted analizar que es un error dentro del codigo.
Los Alumnos tendran que forkear este repo como propio, hacer un issue desde Github con Comentarios refiriendo en que linea esta el error, y como se debe solucionar.
La respuesta sera con el link a ese Fork, y adentro deben estar los issues. Los profesores tenemos que poder ingresar al mismo. Recae en los alumnos asegurarse de que los profesores puedan ingresar.
Tambien pueden editar el Archivo Readme y poner los resultados dentro de sus propios forks.
https://github.com/ExBattou/SimpsonsApp



Respuesta: 

ERROR 1: 
en el archivo Episode.kt -linea 13 a 15- hay un bloque init { return Episode; } fuera de la clase. Eso no compila en Kotlin 
Para arreglarlo, habria que borrar esas 3 líneas 

ERROR 2: 
En el archivo EpisodeRepositoryImpl.kt -linea 8- genera el metodo getEpisodes() pero luego es llamado en EpisodeRepository.kt como get_episodes() -linea23- y en el archivo GetEpisodesUseCase.kt como get_episodes()-linea 13- tambien. 
Esto genera que no compile el archivo 
Para arreglarlo, deberia unificar los nombres y que todos tengan getEpisodes()

ERROR 3: 
En el archivo EpisodeRepositoryImpl.kt -linea 18- se usa SimpsonsApi sin importarlo. Para arreglarlo deberiamos importarlo 

ERROR 4: 
En el archivo DataModule.kt -linea 34 a 37- Retrofit.Builder() no tiene .baseUrl() por lo cual falla al inicializarse
Para arreglarlo deberia ser algo como: .baseUrl("https://thesimpsonsapi.com/") antes de .build()

ERROR 5:
En el archivo EpisodeRemoteMediator.kt -linea 106- el metodo @GET usa la URL completa en lugar de la ruta relativa: 
interface SimpsonsApi {
    @GET("https://thesimpsonsapi.com/api/episodes")
    suspend fun getEpisodes(
        @Query("page") page: Int
    ): EpisodesResponse
}
Para arreglarlo se debe cambiar @GET("api/episodes") y usar el baseUrl del error 4

ERROR 6: 
En el archivo EpisodeRemoteMediator.kt -linea 31- en REFRESH, la pagina se calcula como nextKey - 1 en lugar de volver a la pagina 1
Para eso se debe usar LoadType.REFRESH -> 1

ERROR 7:
En el archivo MainScreen.kt -linea 96- la lista usa LazyRow generando un error de UX al aparecer los episodios apilados horizontalmente, lo cual hace que el contenido sea inutilizable
Ademas, en el mismo archivo ya se usa un Row horizontal para el selector de episodios-lineas 78–97- por lo que tener tambien la lista principal horizontal genera una superposicion confusa de dos scrolls horizontales

ERROR 8: 
En el archivo MainScreen.kt -linea 51 a 53- el view.Model.refreshSeasons() se llama directo en el cuerpo de composable. Esto es un efecto secundario que puede ejecutarse en cada recomposicion sin la necesidad de que este en el composabe 
Para arreglarlo, se deberia mover la logica a un LaunchedEffect

ERROR 9: 
En el archivo MainScreen.kt -linea 18 y 23- hay dos errores: 
Línea 18: MainScreen(FAKE_DATA) no compila (espera un lambda (Int) -> Unit).
Línea 23: busca "Hello $it!", texto que no existe en la UI.
Para arreglarlo, se deberia pasar MainScreen(onNavigateToDetail = {}) y actuaizar las aserciones al contenido real (ej. "The Simpsons Episodes")

ERROR 10: 
En el archivo MainScreen.kt -linea96- los FilterChip del episodio tienen selected = false fijo. con esto nunca muestran seleccion 
Para arreglarlo, hay que guardar el episodio seleccionado en un estado (remember { mutableStateOf(...) }) y usar selected = selectedEpisode == epNum.
