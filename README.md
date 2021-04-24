# JS40ProyectoAPIAxiosGitHubBuscadorEmpleos
40. PROYECTO API de Github Jobs para crear un buscador de Empleos con Axios
```javascript
// 2. Primeros pasos y agregando Axios
// https://cdnjs.com/libraries/axios axios es como un fetch api mejorado
// Promise based HTTP client for the browser and node.js
// Es una librería externa. Existen varias formas de agregar librerías
// una es por medio de enlace en el HTML, instalando por medio de npm.
// Al cargar axios en el html, verificar si esta operativo. Colocando en 
// la consola de chrome axios. Aparece una función.


const formulario = document.querySelector('#formulario');
const resultado = document.querySelector('#resultado');

document.addEventListener('DOMContentLoaded', () => {
    formulario.addEventListener('submit', validarBusqueda);
})

function validarBusqueda(e) {
    e.preventDefault();

    const busqueda = document.querySelector('#busqueda').value;
    if(busqueda.length < 3) {
        mostrarMensaje('Búsqueda muy corta. Añade más información');
        return;// para que no se siga exe el código
    }

    consultarAPI(busqueda);
}


function consultarAPI(busqueda) {
    const githubUrl = `https://jobs.github.com/positions.json?location=${busqueda}`;

    // proxy
    // El método encodeURIComponent() codifica un componente URI (Identificador Uniforme de Recursos) al reemplazar cada instancia de 
    // ciertos caracteres por una, dos, tres o cuatro secuencias de escape que representan la codificación UTF-8 del carácter (solo serán 
    // cuatro secuencias de escape para caracteres compuestos por dos carácteres "sustitutos").
    const url = `https://api.allorigins.win/get?url=${ encodeURIComponent(githubUrl) }`;

    // get es opcional, si se quita es un get
    axios.get(url)
        // console.log(respuesta)
        //console.log(respuesta.data.contents) entrega un valor en string
        .then( respuesta => mostrarVacantes(JSON.parse(respuesta.data.contents)));
        // has been blocked by CORS policy: No 'Access-Control-Allow-Origin'
        // este error es debido a que la API esta protegida
        // CORS policy https://developer.mozilla.org/es/docs/Web/HTTP/CORS
        //  Intercambio de Recursos de Origen Cruzado (CORS)

        // ¿Cómo saltar la protección? se utiliza un proxy y una función de JS encodeURIComponent
        // La respuesta de axios la dará en data:
}

function mostrarVacantes(vacantes) {
    //console.log(vacantes);// entrega un arreglo de objetos
    while( resultado.firstChild) {
        resultado.removeChild(resultado.firstChild);
    }

    // Si vacantes tiene algo
    if(vacantes.length > 0) {
        resultado.classList.add('grid');

        vacantes.forEach(vacante => {
            const { company, title, type, url } = vacante;

            resultado.innerHTML += `
            <div class="shadow bg-white p-6 rounded">
                <h2 class="text-2xl font-light mb-4">${title}</h2>
                <p class="font-bold uppercase">Compañia:  <span class="font-light normal-case">${company} </span></p>
                <p class="font-bold uppercase">Tipo de Contrato:   <span class="font-light normal-case">${type} </span></p>
                <a class="bg-teal-500 max-w-lg mx-auto mt-3 rounded p-2 block uppercase font-xl font-bold text-white text-center" href="${url}">Ver Vacante</a>
        </div>`;
        });
    } else {
        // 4. Mostrar un mensaje cuando no haya vacantes
        const noResultado = document.createElement('p');
        noResultado.classList.add('text-center', 'mt-10', 'text-gray-600', 'w-full');
        noResultado.textContent = 'No hay vacantes, intenta con otro término de búsqueda';
        // Mensaje queda a la izq debido a los grid
        resultado.classList.remove('grid');
        resultado.appendChild(noResultado);
    }
}

function mostrarMensaje(msg) {

    const alertaPrevia = document.querySelector('.alerta');
    if(!alertaPrevia) {
        const alerta = document.createElement('div');
        alerta.classList.add('bg-gray-100', 'p-3', 'text-center', 'mt-3', 'alerta');
        alerta.textContent = msg;

        formulario.appendChild(alerta);

        setTimeout(() => {
            alerta.remove();
        }, 3000);
    }
}
```
