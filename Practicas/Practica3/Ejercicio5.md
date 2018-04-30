Ejercicio 5
======
En un entrenamiento de futbol hay 20 jugadores que forman 4 equipos (cada jugador conoce el equipo al cual pertenece llamando a la función dar_equipo()). Cuando un equipo está listo (han llegado los 5 jugadores que lo componen), debe enfrentarse a otro equipo que también esté listo (los dos primeros equipos en juntarse juegan en la cancha 1, y los otros dos equipos juegan en la cancha 2). Una vez que el equipo conoce la cancha en la que juega, sus jugadores se dirigen a ella. Cuando los 10 jugadores del partido llegaron a la cancha comienza el partido, juegan durante 50 minutos, y al terminar todos los jugadores del partido se retiran (no es necesario que se esperen para salir).

Solución
------
process jugador[i=1..20](){
  int nro_equipo, nro_cancha;
  nro_equipo = dar_equipo();
  Equipo[nro_equipo].await_compañeros(nro_cancha);
  Cancha[nro_cancha].jugar();
}

Monitor Equipo[i=1..4](){
  cond cola;
  int cancha;
  int cant = 0;
  
  procedure await_compañeros(var int nro_cancha){
    cant++;
    if(cant < 4){
      wait(cola);
    }else{
      Coordinador.dar_cancha(cancha);
    }
    nro_cancha = cancha;
    signal(cola);    
  }
}

Monitor Coordinador(){
  int cancha = 1; int nro_equipo = 1;
  
  procedure dar_cancha(var int nro_cancha){
    if(equipo < 2){
      cancha++;
      nro_equipo = 1;
    }
    nro_cancha = cancha
    nro_equipo++;
  }
}

Monitor Cancha[i=1..2](){
  int cant = 0;
  cond jugadores;
  
  procedure jugar(){
    cant++;
    if(cant < 10){
      wait(jugadores)
    }else{
      delay(50*60);
      signal_all(jugadores);
    }
  }
}
