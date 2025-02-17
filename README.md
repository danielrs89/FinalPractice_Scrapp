# Web Scraping

Data scraping is a computer process that uses an application to extract valuable information from a website.

## Example code

Add websites
```java
private static void insertDomain() {
        boolean valid = false;
        String confirmation;
        try {
            do {
                String name = InputManager.leerCadena("Nombre del dominio");
                String url = InputManager.leerCadena("URL del dominio web (Ejemplo => https://www.google.com/)").trim();

                if (url.contains("www") || url.contains("http")) {
                    wP = new webPage(name, url, 0);
                    listWP.add(wP);
                    System.out.println("Dominio agregado correctamente");
                } else {
                    System.out.println("\nURL no válida");
                }

                confirmation = InputManager.leerCadena("¿Quiéres añadir otro dominio? (SI/NO)");
                if (confirmation.equalsIgnoreCase("no")) valid = true;
            } while (!valid);
        } catch (Exception e) {
            System.out.println("Error => " + e.getMessage());
            menu();
        }
    }
```
Most important method download source code and search url inside using Threads

```java
    public static void scrapper() {
        //link to thread and count elapsed time
        Summary.inicio();
        Hilo h = new Hilo();
        h.start();
        Summary.fin();
        Summary.createFile();
        System.out.println("Fin de analizar");
        menuScrapper();
    }

    // Example of thread
    public class Hilo extends Thread {

    @Override
    public void run() {

        try {
            Main.listWP.get(Main.getIndex()).setState(1);
            Scrapp.downloadSourceCode();
            Scrapp.showLinks(Scrapp.emptyFile());
            Main.listWP.get(Main.getIndex()).setState(2);
        } catch (Exception e) {
            System.out.println("Error => " + e.getMessage());
        }
    }
}
```

Download source code for the selected domain
```java
static void downloadSourceCode() {
        BufferedReader br = null;
        FileWriter fr = null;
        File fichero;
        URL url;
        String line;

        try {
            System.setProperty("java.protocol.handler.pkgs", "com.sun.net.ssl.internal.www.protocol");
            //I get the url ***NEEDS TYPE URL
            url = new URL(listWP.get(getIndex()).getUrl());
            //I create a file named
            fichero = new File(
                    listWP.get(getIndex()).getName() + ".dat");
            //read codeSource from url
            br = new BufferedReader(new InputStreamReader(url.openStream()));
            //I declare where I want to write
            fr = new FileWriter("src\\" + fichero);
            
            while ((line = br.readLine()) != null) {
                fr.write(line);
            }
        } catch (Exception e) {
            System.out.println("Error => " + e.getMessage());
        } finally {
            try {
                if (br != null && fr != null) {
                    br.close();
                    fr.close();
                }
            } catch (Exception e) {
                System.out.println("Error => " + e.getMessage());
            }
        }
    }
```
method used to extract url
search the source code of the domain for matches to the <a href=""></a> pattern.
```java
    static String emptyFile() {
        try {
            File fichero = new File(listWP.get(getIndex()).getName());//I create a file named
            BufferedReader lector = new BufferedReader(new FileReader("src\\"+ fichero + ".dat"));
            StringBuilder cadena = new StringBuilder();
            String line;

            while ((line = lector.readLine()) != null) {
                cadena.append(line);
            }

            lector.close();
            content = cadena.toString();
            return content;
        } catch (Exception e) {
            System.out.println("Error => " + e.getMessage());
        }
        return null;
    }
```



