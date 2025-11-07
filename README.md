
# Interfaces Inteligentes seminario 3

1. **Qué funciones se pueden usar en los scripts de Unity para llevar a cabo traslaciones, rotaciones y escalados.**

* Translaciones:
    * Translate
* Rotaciones:
    * LookAt
    * Rotate
    * RotateAround
* Escalados:
    * Usar propiedad localScale

2. **Como trasladarías la cámara 2 metros en cada uno de los ejes y luego la rotas 30º alrededor del eje Y?. Rota la cámara alrededor del eje Y 30ª y desplázala 2 metros en cada uno de los ejes. ¿Obtendrías el mismo resultado en ambos casos?. Justifica el resultado**

```C#
transform.Translate(2, 2, 2);
transform.Rotate(0, 30, 0);
```

No se obtiene el mismo resultado ya que translate usa los ejes locales del objeto, que cambian al realizar la rotación.

3. **Sitúa la esfera de radio 1 en el campo de visión de la cámara y configura un volumen de vista que la recorte parcialmente.**

Modicado Clipping Plane (Far) a `4.5` para que recorte parcialmente la esfera de radio 1

![|400](https://i.imgur.com/zXdWjf3.jpeg)

4. **Sitúa la esfera de radio 1 en el campo de visión de la cámara y configura el volumen de vista para que la deje fuera de la vista.**

Modicado Clipping Plane (Far) a `3` para que quede fuera del volumen de la vista a la esfera de radio 1

![|400](https://i.imgur.com/tL32zTA.jpeg)

![|400](https://i.imgur.com/fLi9oxX.jpeg)

5. **Como puedes aumentar el ángulo de la cámara. Qué efecto tiene disminuir el ángulo de la cámara.**

 Aumentando el FOV (Field of View). Tiene efecto de zoom, haciendo que todo se vea más grande

6. **Es correcta la siguiente afirmación: Para realizar la proyección al espacio 2D, en el inspector de la cámara, cambiaremos el valor de projection, asignándole el valor de orthographic**

Sí, la afirmación es correcta.

En Unity, el componente **Camera** puede proyectar la escena en dos modos distintos:

- **Perspective** (por defecto): simula la visión humana, donde los objetos lejanos se ven más pequeños.
- **Orthographic**: elimina el efecto de perspectiva, manteniendo el mismo tamaño para todos los objetos sin importar su distancia.

Por tanto, **si queremos proyectar la escena en un espacio 2D**, debemos cambiar en el **Inspector de la cámara** el campo **Projection** de `Perspective` a **`Orthographic`**.  
Esto hace que los rayos de visión sean **paralelos** en lugar de converger hacia un punto, lo cual es ideal para juegos o visualizaciones 2D, ya que las coordenadas en pantalla se corresponden linealmente con las coordenadas del mundo.

7. **Especifica las rotaciones que se han indicado en los ejercicios previos con la utilidad quaternion.**

La rotación en Euler de (0, 30, 0) equivale a un cuaternión de (0.00000, 0.25882, 0.00000, 0.96593)

8. **¿Como puedes averiguar la matriz de proyección en perspectiva que se ha usado para proyectar la escena al último frame renderizado?.**

Usando Camera.projectionMatrix

9. **¿Como puedes averiguar la matriz de proyección en perspectiva ortográfica que se ha usado para proyectar la escena al último frame renderizado?.**

De la misma manera, con Camera.projectionMatrix

10. **¿Cómo puedes obtener la matriz de transformación entre el sistema de coordenadas local y el mundial?.**

Usando Transform.localToWorldMatrix

11. **Cómo puedes obtener la matriz para cambiar al sistema de referencia de vista**

Usando Camera.worldToCameraMatrix

12. **Especifica la matriz de la proyección usado en un instante de la ejecución del ejercicio 1 de la práctica 1.**

Tenemos a partir de estos parámetros de la cámara:

![](https://i.imgur.com/bNNZD7W.png)


Usando el siguiente script:

```c#
    Camera camera = Camera.main; // Accede a la cámara principal
        Matrix4x4 projectionMatrix = camera.projectionMatrix; // Obtén la matriz de proyección
        Debug.Log(projectionMatrix); // Imprime la matriz en la consola para visualizarla
````

Obtenemos la matriz de proyección por consola:

```text
0.85086	0.00000	0.00000	0.00000
0.00000	1.73205	0.00000	0.00000
0.00000	0.00000	-1.00060	-0.60018
0.00000	0.00000	-1.00000	0.00000
```

13. **Especifica la matriz de modelo y vista de la escena del ejercicio 1 de la práctica 1.**

Dado nuestro cubo en esta posición:

![](https://i.imgur.com/G2fpPBJ.png)

su matriz de modelo sería:

```text
1 0 0 0
0 1 0 0.67
0 0 1 0
0 0 0 1
```

Por otro lado, la matriz de vista sería:

```text
0.85086	0.00000	0.00000	0.00000
0.00000	1.73205	0.00000	0.00000
0.00000	0.00000	-1.00060	-0.60018
0.00000	0.00000	-1.00000	0.00000
```

Esto se ha sacado a partir del siguiente script atribuido a la cámara:

```c#
    Camera camera = Camera.main;
    Matrix4x4 viewMatrix = camera.worldToCameraMatrix;
    Debug.Log(viewMatrix);
```

14.**Aplica una rotación en el start de uno de los objetos de la escena y muestra la matriz de cambio al sistema de referencias mundial.**

Obtenemos la matriz de cambio al sistema de referencias mundial del cubo al rotarlo 45 grados en el eje Y:
```text
0.70711	0.00000	0.70711	0.00000
0.00000	1.00000	0.00000	0.67000
-0.70711	0.00000	0.70711	0.00000
0.00000	0.00000	0.00000	1.00000
```

Con el siguiente script:

```c#
void Start()
    {
        // Se aplica una rotación de 45 grados en el eje Y
        transform.Rotate(0, 45, 0);

        Matrix4x4 worldMatrix = transform.localToWorldMatrix;

        Debug.Log("Matriz de cambio al sistema de referencias mundial:");
        Debug.Log(worldMatrix);
    }
```

15. **¿Como puedes calcular las coordenadas del sistema de referencia de un objeto con las siguientes propiedades del Transform:?: 
 Position (3, 1, 1), Rotation (45, 0, 45)**

Se puede hacer construyendo una matriz de transformación global a partir de las propiedades del objeto dadas por transform:

```c#
    transform.position = new Vector3(3, 1, 1);
    transform.rotation = Quaternion.Euler(45, 0, 45);

    Matrix4x4 worldMatrix = transform.localToWorldMatrix;

    Debug.Log("Matriz de transformación:");
    Debug.Log(matrix);
```

16. **Crea una escena en Unity con los siguientes elementos: cámara principal, plano base (como suelo) y tres cubos de distinto color (rojo, verde, azul) colocados en posiciones distintas en el espacio. Realiza un pequeño script de depuración adjunto a la cámara que permita visualizar en consola o en pantalla las matrices de transformación (Model, View, Projection) y sus resultados sobre un vértice de cada cubo.**

Lo que hemos hecho para resolver el ejercicio es crear los 3 cubos, situarlos en distintas posiciones y añadirle a la cámara el siguiente Script:

```C#
using UnityEngine;

public class CameraDebugMatricesConsole : MonoBehaviour
{
    [Header("Referencias a los cubos")]
    public Transform cubeRed;
    public Transform cubeGreen;
    public Transform cubeBlue;

    [Header("Opciones de depuración")]
    public bool logEveryFrame = false;     // Si es true, muestra cada frame
    public KeyCode logKey = KeyCode.Space; // Si es false, muestra al pulsar la tecla

    private Camera cam;

    void Awake()
    {
        cam = GetComponent<Camera>();
        if (cam == null)
        {
            Debug.LogError("Este script debe estar en un GameObject con Camera.");
        }
    }

    void Update()
    {
        if (cam == null) return;

        if (logEveryFrame || Input.GetKeyDown(logKey))
        {
            // Matrices de la cámara
            Matrix4x4 viewMatrix = cam.worldToCameraMatrix;
            Matrix4x4 projMatrix = cam.projectionMatrix;

            System.Text.StringBuilder sb = new System.Text.StringBuilder();

            sb.AppendLine("=== MATRICES DE LA CÁMARA ===");
            sb.AppendLine("View (world -> camera):");
            sb.AppendLine(viewMatrix.ToString());
            sb.AppendLine();
            sb.AppendLine("Projection (camera -> clip):");
            sb.AppendLine(projMatrix.ToString());
            sb.AppendLine();

            // Procesar cada cubo
            if (cubeRed != null)
                AppendCubeInfo(sb, cubeRed, "Cubo ROJO", viewMatrix, projMatrix);

            if (cubeGreen != null)
                AppendCubeInfo(sb, cubeGreen, "Cubo VERDE", viewMatrix, projMatrix);

            if (cubeBlue != null)
                AppendCubeInfo(sb, cubeBlue, "Cubo AZUL", viewMatrix, projMatrix);

            Debug.Log(sb.ToString());
        }
    }

    void AppendCubeInfo(System.Text.StringBuilder sb, Transform cube, string label,
                        Matrix4x4 viewMatrix, Matrix4x4 projMatrix)
    {
        MeshFilter mf = cube.GetComponent<MeshFilter>();
        if (mf == null || mf.sharedMesh == null) return;

        Mesh mesh = mf.sharedMesh;
        if (mesh.vertexCount == 0) return;

        // Tomamos el primer vértice del cubo (en espacio local)
        Vector3 vLocal = mesh.vertices[0];

        // Matriz Model (object -> world)
        Matrix4x4 modelMatrix = cube.localToWorldMatrix;

        // Transformaciones sucesivas
        Vector3 vWorld = modelMatrix.MultiplyPoint3x4(vLocal);     // Model
        Vector3 vView = viewMatrix.MultiplyPoint3x4(vWorld);       // View
        Vector3 vClip = projMatrix.MultiplyPoint(vView);           // Projection

        sb.AppendLine("=== " + label + " ===");
        sb.AppendLine("Model (object -> world):");
        sb.AppendLine(modelMatrix.ToString());
        sb.AppendLine();
        sb.AppendLine("Vértice (local): " + vLocal);
        sb.AppendLine("Tras Model (world): " + vWorld);
        sb.AppendLine("Tras View (camera space): " + vView);
        sb.AppendLine("Tras Projection (clip space): " + vClip);
        sb.AppendLine();
    }
}

```

Después, en el inspector de la propia cámara, añadimos las referencias a los cubos y marcamos la opción `Log Every Frame` para que me aparezca la información en cada frame por consola. El resultado se muestra por consola como se muestra en las siguientes imágenes: 

![](https://i.imgur.com/qbySHVo.jpeg)

![](https://i.imgur.com/ksz8gwa.jpeg)

17. **Dibujar en un programa de dibujo el recorrido de las coordenadas de un vértice específico del cubo rojo: Local → World → Camera/View → Clip → NDC → Viewport. Indicar cómo cambia su valor en cada espacio. Aplicar la transformación manualmente a un punto (por ejemplo, el vértice (0.5, 0.5, 0.5)) y registrar los resultados paso a paso.**


![](https://i.imgur.com/sHrUP6T.png)

La idea es: mismo punto físico, pero sus coordenadas cambian porque estás cambiando el sistema de referencia y aplicando proyección.

Qué es cada espacio, explicado:

1. **Local (Object Space)**  
	Coordenadas dentro del cubo.
	- Ejemplo: vértice (0.5, 0.5, 0.5) es una esquina del cubo en su propio sistema.
2. **World Space**  
	Coordenadas en el mundo de la escena.
	- Aplicas la Model Matrix (posición, rotación y escala del cubo).
	- Sirve para “colocar” el cubo en la escena.
3. **Camera / View Space**  
	Coordenadas vistas desde la cámara.
	- Aplicas la View Matrix (posición y orientación de la cámara).
	- Es como mover el mundo para que la cámara quede en el origen mirando hacia adelante.
4. **Clip Space**  
	Resultado de aplicar la Projection Matrix (perspectiva u ortográfica).
	- Aquí ya se ha aplicado FOV, planos near/far, etc.
	- El punto está en forma homogénea: (x, y, z, w).
5. **NDC (Normalized Device Coordinates)**  
	- Divides por w:
	- $(x_{ndc}, y_{ndc}, z_{ndc}) = \left(\frac{x}{w}, \frac{y}{w}, \frac{z}{w}\right)$
	- Ahora las coordenadas están normalmente entre \[-1, 1\].
6. **Viewport (Screen Space)**  
	Mapeas de \[-1, 1\] → píxeles de la pantalla.
	- Si la pantalla es de 800×600:
		- $x_{screen} = \frac{x_{ndc} + 1}{2} \cdot 800$
		- $y_{screen} = \frac{y_{ndc} + 1}{2} \cdot 600$

---

18. **Mover o rotar uno de los cubos y mostrar cómo cambian los valores de su matriz de modelo. Rotar la cámara y mostrar cómo se modifica la matriz de vista. Cambiar entre proyección ortográfica y perspectiva y comparar las diferencias numéricas en la matriz de proyección.**

Posición original:

![](https://i.imgur.com/SE5EIOr.jpeg)

Posición tras mover el cubo de posición:

![](https://i.imgur.com/11VRLgn.jpeg)

Matriz original de la cámara:

![](https://i.imgur.com/0alZnFz.jpeg)

Matriz tras haber rotado la cámara:

![](https://i.imgur.com/NZ8urwU.jpeg)

Proyección en perspectiva:

![](https://i.imgur.com/qQbWKxP.jpeg)

Proyección en ortográfica:

![](https://i.imgur.com/tLtePcg.jpeg)

