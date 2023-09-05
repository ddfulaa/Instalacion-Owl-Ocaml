# Instalación de Ocaml y Owl

Para instalar Ocaml simplemente se requiere del gestor Opam, que se encargará de instalar Ocaml. Nota: La siguiente guía supondrá que dispone de una distribución de linux basada en Ubuntu (en mi caso Linux Mint).

```bash
sudo apt-get install opam
```

Luego de la instalación es necesario ejecutar el siguiente comando para inicializar las configuraciones de Opam:

```bash
opam init
```

Para más información de los comandos de opam vea el siguiente enlace: 

[https://opam.ocaml.org/doc/Usage.html](https://opam.ocaml.org/doc/Usage.html)

Para instalar Owl debe tener instalado

- build-essentials : Que permite tener el compilador básico de c y otras cosillas para desarrolladores
- libopenblas-dev: Que contiene la información para complilar OpenBlas, que es un paquete de algoritmos optimizados de álgebra lineal básica
- liblapacke-dev:  Que contiene la información para compilar lapacke, un paquete de algoritmos de álgebra lineal y que usa OpenBlas
- pkg-config: que es un software que provee una interfaz unificada para llamar bibliotecas 
 instaladas cuando se está compilando un programa a partir del código fuente.

```bash
sudo apt update
sudo apt-get install build-essentials libopenblas-dev liblapacke-dev pkg-config
```

Luego de instalar lo anterior, puede proceder a instalar owl desde opam.

```bash
opam install owl
```

También podría necesitar los siguientes paquetes: owl-top (para ejecutar owl en una CLI), owl-ode (paquete para Ecuaciones diferenciales ordinarias), owl-opt (paquete para Optimización), utop (una CLI/REPL que funciona mejor que la que trae Ocaml por defecto)

```ocaml
opam install owl-top owl-ode owl-opt utop
```

Para hacer que Owl funcione correctamente desde utop, debemos configurar el archivo .ocamlinit en el directorio del usuario (~). Para ello simplemente lo creamos y escribimos lo siguiente en el:

```ocaml
#use “topfind”;;  (*Sin esto no podemos usar #require*)

Topfind.log := ignore;;

#thread;;

#require “owl-top”;;

open Owl;; (* Para cargar Owl cada vez que se inicia utop*)
```

Después de lo anterior tendríamos todo listo para comenzar a usar Owl desde utop.  Para verificar que esté funcionando correctamente, puede correr el siguiente código una vez que abra utop:

```ocaml
Maths.sqrt(100.)
```

Adicionalmente a lo anterior, también podría querer usar los notebook de Jupyter con ocaml y owl, para ello se requiere instalar el paquete owl-jupyter de Opam. Puede visitar [https://github.com/akabe/ocaml-jupyter](https://github.com/akabe/ocaml-jupyter) , aunque a mí no me funcionó tal como estaba escrito allí. Si ese es su caso, entonces haga lo siguiente:

Para comenzar, instale los paquetes necesarios para instalar jupyter.

```bash
# Para debian/ubuntu/linux mint
sudo apt-get install -y zlib1g-dev libffi-dev libgmp-dev libzmq5-dev libplplot-dev
```

Debe tener instalado pip. Ejecute

```bash
sudo apt-get install python3-pip
sudo apt-get install jupyter
pip3 install jupyter
opam install owl-jupyter jupyter
#Genere el archivo del kernel que va a agregar a jupyter
ocaml-jupyter-opam-genspec
#Agregue Ocaml como kernel para jupyter, --user es opcional, pero a mí no me funciona si no lo pongo
jupyter kernelspec install --user --name "ocaml-jupyter-$(opam var switch)" "$(opam var share)/jupyter"
```

Ahora revise la configuración del archivo .ocamlinit, aunque debería funcionar con #use “topfind”, muchas veces jupyter no lo reconoce y por tanto es imposible cargar Owl (Aparece un error que dice que no se encuentra el archivo topfind y al intentar cargar owl o cualquier otra cosa también muestra un error). Para solucionar esto, agregue al inicio del archivo .ocamlinit lo siguiente (recuerde que debe tener lo que se usó anteriormente)

```ocaml
let () = 
  try Topdirs.dir_directory (Sys.getenv "OCAML_TOPLEVEL_PATH")
  with Not_found -> ()
;;
```

Puede agregar al inicio de cada notebook el siguiente código para que todo se ejecute normalmente. Note que también puede agregarlo al archivo .ocamlinit

```ocaml
#require "owl-jupyter"
open Owl_jupyter
```

Después de esto, puede abrir un notebook y comprobar si funciona el mismo código que se usó anteriormente para verificar que owl estuviese bien configurado.

Por último, le recomiendo leer el tutorial para usar owl: 

[https://ocaml.xyz/tutorial/toc.html](https://ocaml.xyz/tutorial/toc.html)

y la siguiente página, que además de tener información de paquetes para usar ocaml en Machine Learning/ Ciencia de datos, tiene mucha más información acerca de Ocaml

[https://ocamlverse.github.io/content/scientific.html](https://ocamlverse.github.io/content/scientific.html)
