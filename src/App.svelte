<script>
  import * as d3 from "d3"
  import {onMount} from "svelte"

  
      // Importa las imágenes desde tu carpeta public/images
    import imagen1 from '../public/images/MX.png';
    import imagen2 from '../public/images/Mwarap.png';
    import imagen3 from '../public/images/MIg.png';
    import imagen4 from '../public/images/Mtiktok.png';
    import imagen5 from '../public/images/MYt.png';
    import imagen6 from '../public/images/relojC.png';
    import imagen7 from '../public/images/relojR.png';
    import imagen8 from '../public/images/relojV.png';
    
    // Define una variable para almacenar los valores del bucle
    let valores = [];
    let valorActual = 0;
    


//div texto descripcion
let texto = "Representación de los datos:";




// Función para cambiar el valor actual (por ejemplo, incrementar en 1)
      function cambiarAutomaticamente() {
        setInterval(() => {
            valorActual = (valorActual + 1) % 12;
        }, 1000); // Cambia cada 1000 ms (1 segundo)
    }

    // Llama a la función al cargar la página
    cambiarAutomaticamente();

    // Agrega más imágenes según sea necesario
/* Array donde guardaremos la data */
    let personas = []
    // Define un arreglo con los datos de las imágenes y sus textos
    const imagenes2 = [
        { src: imagen6, texto: 'Masculino' },
        { src: imagen7, texto: 'Femenino' },
        { src: imagen8, texto: 'No binario' },
        
        // Agrega más objetos para más imágenes
    ];
    const imagenes = [
        { src: imagen1, texto: 'X' },
        { src: imagen2, texto: 'Whatsapp' },
        { src: imagen3, texto: 'Instagram' },
        { src: imagen4, texto: 'Tiktok' },
        { src: imagen5, texto: 'YouTube' },
        // Agrega más objetos para más imágenes
    ];

  function rotarAguja(tiempoCel) {
        // Calcular el ángulo en función de tiempoCel
        const maxTiempoCel = d3.max(personas, d => d.tiempoCel);
        const minTiempoCel = 0;
        //d3.min(personas, d => d.tiempoCel);
        const range = 12;
        const normalizedTiempoCel = (tiempoCel - minTiempoCel) / range; // Normalizar tiempoCel
        const angle = normalizedTiempoCel * 360; // Convertir a grados

        return angle;
    }
  
  let imgRelojes = d3
    .scaleOrdinal()
    .domain(["F", "M", "NB"])
    .range(["images/relojR.png", "images/relojC.png", "images/relojV.png"])

  /* 1. Escala para bordes */
  let grosor = d3.scaleLinear().range([0, 3])

  /* 2. Escala para genero */
  let colorGenero = d3
    .scaleOrdinal()
    .domain(["F", "M"])
    .range(["#ffc0cb", "#c0f9ff"])

  /* 3. Escala de colores para apps */

  let imgApps = d3
    .scaleOrdinal()
    .domain(["Twitter", "Instagram", "Tik Tok", "YouTube", "Whatsapp"])
    .range(["images/MX.png", "images/MIg.png", "images/Mtiktok.png", "images/MYt.png", "images/Mwarap.png"])

  let colorGeneros = d3
    .scaleOrdinal()
    .domain(["F", "M", "NB", "O"])
    .range(["rgba(255, 192, 203, 0.5)", "rgba(155, 176, 249, 0.5)", "rgba(163, 249, 155, 0.5)", "rgba(255, 0, 0, 0.5)"])

  /* 4. Escala para altura */
  let radioAltura = d3.scaleLog().range([0, 30])

  onMount(() => {
    d3.csv("/data/personas.csv", d3.autoType).then(data => {
      console.log(data)

      /* Actualizamos dominio con la data de edad */
      /*
      let minMaxCant = d3.extent(data, d => d.cantApps)
      radio = radioAltura.domain(minMaxCant)
      */
      /* Actualizamos dominio y rango con la data de altura */
      
      //let minMaxGrosor = d3.extent(data, d => d.tiempoCel)
      //grosor = grosor.domain(minMaxAltura).range([0, 10])
      personas = data
      
    })
    // background-image: gradient-linear (to right, #FFC0CB, #9BB0F9);
  })
</script>

<main>
  <div>
  </div>
  <div class="header">
   
    <!-- aca podemos poner una fotito -->
    <!-- <img class = "titulo" src = "public/images/tiempo.png" alt = "tiempo"/> -->
    <h3 class="headline">

      <b>Uso de apps mobiles</b>
      Encuesta visualización de datos
    </h3>
    <p class="bajada">Representación gráfica de los datos obtenidos</p>
  </div>

  <!-- Conedor de las entidades -->
  <div class="container">
    <!-- Iteramos la data para visualizar c/ entidad -->
    {#each personas as p}
    <div class="person-container" style="height: 230px;">
        <div
  class="person"
  style="border-width: 5px; 
         width: {2 * radioAltura(p.cantApps)}px; 
         height: {2 * radioAltura(p.cantApps)}px; 
         background-image: url('{imgApps(p.mayorTiempoApp)}'); /* URL de la imagen */
         background-size: cover; /* Ajusta el tamaño de la imagen */
         /* box-shadow: 0 0 20px 10px {colorGeneros(p.genero)}; */
         border-color: {p.pago ? "gold" : "black"};
         position: relative;
         margin-top: auto">
         <img src = {imgRelojes(p.genero)} alt = "aguja" style="width: 100%; height: 100%; transform: rotate({rotarAguja(p.tiempoCel)}deg); position: absolute; top: 0; left: 0; z-index: 1;"/>
        <img src = "images/lineasReloj3.png" alt = "lineas" style="width: 100%; height: 100%; position: absolute;"/>
        </div>
        
        <p class="name">
          <br />

          <b>{p.nombre}</b>
          <br />
           {p.appFav} 
        </p>
      </div>
    {/each}
  </div>
  <div>
     <hr>
  </div>




 <!-- arranca todo lo relacionado al footer-->
  <div class = "footer" style="position: relative;display: flex;flex-direction:column">
    <br>
    <div class="texto"><b>{texto}</b></div>
    <div class="bordesito" style="border:5px ;border-radius:20px;display:flex;flex-direction:row;justify-content:space-between" > 


      <!-- esto es primer div del row  -->
    <div class="bordesito" on:mouseenter={() => texto = "App mas usada por el encuestado:"} on:mouseleave={() => texto = "Representación de los datos:"}>
    <div style="display:flex;flex-direction:row; width:min-content;">
      {#each imagenes as imagen}
          <div class="grilla-item" style="padding: 5px;margin-top: 5px">
              <img src={imagen.src} alt="Imagen">
              <p class="textito">{imagen.texto}</p>

          </div>
      {/each}
    </div>
  </div>


 <!-- segundo div del row -->
  <div class="bordesito"style="width:min-content"on:mouseenter={() => texto = "Género del encuestado:"} on:mouseleave={() => texto = "Representación de los datos:"}>
    
    <div style="display:flex;flex-direction:row; width:min-content;" >
      {#each imagenes2 as imagen}
    <div class="grilla-item" style="padding: 5px;margin-top: 5px">
      <div style="width:100px;height:100px;">
        <div style="
            border: 1px solid black;
            width: 100px;
            height: 100px;
            border-radius: 50%;
            position: absolute;
        "></div>
            <img src={imagen.src} alt="Imagen" style="width:100px;height:100px;position: absolute;">
            <img src="images/lineasReloj3.png" alt="lineas" style="width: 100px; height: 100px; position: absolute;">
          </div>
        <p class="textito">{imagen.texto} </p>
    </div>
{/each}
</div>
    </div>






     <!-- tercer div del row -->
<div class="bordesito" style="width:min-content;display:flex;flex-direction:row;gap:5px"on:mouseenter={() => texto = "El usuario pagó por una app?"} on:mouseleave={() => texto = "Representación de los datos:"}>
  
  <div class="grilla-item"style="margin-top: 5px">
    <div style="display:flex; flex-direction: column;text-align: center;align-items:center">
      <div style="width: 90px;height: 90px; border: 5px solid black; border-radius:50%;"></div>
      <p class="textito" style= "width:100px">Si</p>
      </div>
  </div>
  <div class= "grilla-item" style="margin-top: 5px">
    <div style="display:flex; flex-direction: column; text-align: center;align-items:center">
      <div style="width: 90px;height: 90px; border: 5px solid gold; border-radius:50%;"></div>
      <p class="textito" style="width:100px">No</p>
      </div>
  </div>
  </div>




 <!-- ultimo div del row -->
    <div class="bordesito"style="width:min-content;display:flex;flex-direction:row;gap:5px"on:mouseenter={() => texto = "Horas diarias en pantalla del encuestado:"} on:mouseleave={() => texto = "Representación de los datos:"}>
  <div  class="grilla-item"style="display: flex;flex-direction:column;padding:5px;margin-top: 5px">
    <div class="reloj-container" style="width: 100px; height: 100px;">
      <div style="width:100px;height:100px;border: 1px solid black; border-radius: 50%; position:absolute">
          <img src="public/images/reloj.png" alt="aguja" class="aguja" style="width: 100px; height: 100px; transform: rotate({rotarAguja(valorActual)}deg); position: absolute; top: 0; left: 0; z-index: 1;">
          <img src="images/lineasReloj3.png" alt="lineas" style="width: 100px; height: 100px; position: absolute; top: 0; left: 0; z-index: 0;">
      </div> 
  </div>
  <p class="textito" style= "text-align: center">{valorActual} horas</p>
  </div>
</div>
    </div> 

  <p style="text-align:center">El tamaño de los relojes es representado según la cantidad de apps que tiene el encuestado.<br>Por debajo del nombre se encuentra su App favorita.</p>
  <br>
  <p style="text-align:center">Trabajo realizado por Felipe Caracoix y Juan Ignacio Castore.</p>


  </div>
</main>


<style>
  .texto {
        /* Estilo para el div con la clase "texto" */
        
        top: 20px;
        left: 250px;
        padding-left: 25px;
        padding-bottom: 5px;       
       
    }
  .bordesito {
    
    border: double 5px transparent;
  border-radius: 30px;
  background-image: linear-gradient(white, white), 
                    linear-gradient(to right, rgb(255, 217, 0), gold);
  background-origin: border-box;
  background-clip: content-box, border-box;
  transition: background-color 0.3s ease;
    cursor: pointer;
}
  .textito {
    margin-top: 0px;
    text-align: center;
    width: 100px;
    
  }
  hr {
            height: 2px;
            background-image: linear-gradient(to right, black, black);
            border: none; /* Elimina la línea predeterminada del hr */
        }
  .grilla {
        margin: 20px;
        
        display: grid;
        grid-template-columns: repeat(5, 1fr); /* Cambia el número de columnas según tus necesidades */
        gap: 5px; /* Espacio entre elementos */
    } 

    .grilla-item {
        display: flex;
        position: relative;
        justify-content: center;
        align-items: center;
        width: 100%;
        flex-direction: column;
        text-align:center;
        align-items: center;
    }

    .grilla-item img {
        width: 100px;
        height: 100px;
        display: block;
    }

    .grilla-item p {
        bottom: 0;
        left: 0;
        width: 100%;
        color: black;
        padding: 5px;
        margin: 0;
        font-size: 14px;
    }
   

  /* Define the breathing animation */
  @keyframes breathe {
    0% {
      transform: scale(1);
    }
    50% {
      
      transform: scale(1.1);
    }
    100% {
      
      transform: scale(1);
    }
  }

  /* Aplica la animación al elemento .person */
  .person:hover {
    animation: breathe 2s infinite alternate;
  }

  
    .header {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      margin-top: 50px;
      margin-bottom: 80px;
    }
  .headline {
    font-size: 30px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 20px;
  }
  .bajada {
    font-size: 18px;
    font-weight: normal;
    text-align: center;
    margin: 10px;
  }
  .headline b {
    display: block;
  }
  .container {
    display: grid;
    grid-template-columns: repeat(5, minmax(180px, 1fr));
    gap: 20px;
    justify-content: center;
    align-items: center;
    margin: 20px;
    

  }
  .person-container {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    flex: 180px 0 0;
  }
  .person {
    width: 100px;
    height: 100px;
    border: 10px solid black;
    border-radius: 50%;
    box-sizing: border-box;
    /* margin-top: 100%; */
  }
  .name {
  font-size: 14px;
  color: rgb(65, 65, 65);
  font-weight: normal;
  text-align: center;
  
}
</style>
