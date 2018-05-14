# Ejercicio 7

En una estación de comunicaciones se cuenta con 10 radares y una unidad de procesamiento que se encarga de procesar la información enviada por los radares. Cada radar repetidamente detecta señales de radio durante 15 segundos y le envía esos datos a la unidad de procesamiento para que los analice. Los radares no deben esperar a ser atendidos para continuar trabajando.

```c++
process radar[id_radar=1..10](){
  String info;
  while(true){
    info = generar_info();
    buffer!info(info);
  }
}

process buffer(){
  String info, datos; queue cola;
  if(); radar[*]?info(info)-->
    cola.push(info);
  [](); unidad_procesamiento?hay_datos()-->
    unidad_procesamiento!datos(cola.pop());
  end if
}

process unidad_procesamiento(){
  String datos;
  while(true){
    buffer!hay_datos();
    buffer?datos(datos);
    analizar_datos(datos);
  }
}
```
