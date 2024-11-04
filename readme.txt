Para ejecutar el script de Puppeteer en tu entorno local, sigue estos pasos:

1. **Instala Node.js**: Asegúrate de tener Node.js instalado en tu máquina. Puedes descargarlo desde [nodejs.org](https://nodejs.org/).

2. **Crea un nuevo directorio para tu proyecto**:
   ```bash
   mkdir mi-proyecto-puppeteer
   cd mi-proyecto-puppeteer
   ```

3. **Inicializa un nuevo proyecto de Node.js**:
   ```bash
   npm init -y
   ```

4. **Instala Puppeteer**:
   ```bash
   npm install puppeteer
   ```

5. **Crea un archivo llamado `script.js`** y copia el siguiente código:
   ```javascript
   const puppeteer = require('puppeteer');

   async function ejecutarScript() {
       const browser = await puppeteer.launch({ args: ['--no-sandbox'], headless: false });
       const page = await browser.newPage();
       await page.goto('https://cloakmy.org/');
       console.log('Página cargada.');

       const mensajeTexto = '456373';

       await page.evaluate((mensaje) => {
           const iframe = document.getElementById('message_ifr');
           if (iframe) {
               const iframeDoc = iframe.contentDocument || iframe.contentWindow.document;
               iframeDoc.body.innerHTML = mensaje;
               console.log('Mensaje colocado en el iframe.');

               const sendButton = document.querySelector('#submit');
               if (sendButton) {
                   sendButton.click();
                   console.log('Botón de enviar clicado.');
               } else {
                   console.log('Botón de enviar no encontrado.');
               }
           } else {
               console.log('Iframe no encontrado.');
           }
       }, mensajeTexto);

       // Esperar un momento para que se procese la respuesta
       await page.waitForTimeout(3000);

       const elementos = await page.evaluate(() => {
           const responseBox = document.querySelector('.sendsecure.box');
           return responseBox ? responseBox.innerHTML : 'No se encontró la respuesta del envío';
       });

       console.log('Respuesta:', elementos);
       await browser.close();
   }

   ejecutarScript();
   ```

6. **Ejecuta el script**:
   ```bash
   node script.js
   ```

Esto debería abrir un navegador y ejecutar el script en la página especificada.