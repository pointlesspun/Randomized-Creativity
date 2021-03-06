
<img src="https://upload.wikimedia.org/wikipedia/commons/9/9b/First_Steps_%28Millet%29.jpg" style="object-fit:cover" width="100%" height="256px"/>

First steps 
=============================

### [[Top](../readme.md)] [[Next](002-updating-the-editor.md)]

The first goal is to simply drawing a grid. More specifically can we vary the shape of the grid tiles in a meaningful way like in [renowned explorers](https://www.renownedexplorers.com/) or [Townscaper](https://store.steampowered.com/app/1291340/Townscaper), or [this science paper](http://peterwonka.net/Publications/pdfs/2014.TOG.Chihan.QuadExploration.final.pdf), and [a nice demo](https://twitter.com/osksta/status/1147881669350891521?lang=en).

Even smaller for the first step we'll just draw a simple mesh at runtime in Unity. 

To create and draw a mesh at runtime in Unity one has to:

* create an empty GameObject
* add a MeshFilter 
* add MeshRenderer 
* implement a Mesh generation object

To make sure the mesh is indeed as expected, a rotation behavior is added to make the object rotate every second.

To define a mesh, a `MeshDefinition` class is added which simply looks like this:

```csharp
[Serializable]
public class MeshDefinition
{
    public Vector3[] vertices;
    public Vector2[] uv;
    public int[] triangles;

    public bool IsValid() => vertices.Length >= 3 && triangles.Length >= 3 && uv.Length >= 3;
}
```


Although subject to change, the code the core of the mesh generation is following call in `MeshGenerator`:

```csharp
private void UpdateMeshDefinition(Mesh mesh, MeshDefinition definition)
{
    mesh.vertices = definition.vertices;
    mesh.uv = definition.uv;
    mesh.triangles = definition.triangles;

    mesh.RecalculateNormals();
    mesh.RecalculateBounds();
    mesh.RecalculateTangents();
}
```

The results should show up as below:

<center>
    <img src="Images/SimpleMeshTriangle.png" alt="A simple runtime generated triangle" width="640"/>
</center>

## Scope Creep (next step considerations)

There are a couple of shortcomings with this approach, which could be added:

* No editor interactivity, you can't see the mesh as you're changing it in the editor
* The center of the mesh is defined by the definition. I'm not sure if that's ok or if an anchor point should be defined... I'm inclined to say the former, ie it's up to the user to fix the input data, BUT it would be nice and more user friendly if this was adjusted for the user. For now there's no intention to address this.
* Meshes are not doublesided. Same arguments as in the previous point apply.

---
### [[Top](../readme.md)] [[Next](002-updating-the-editor.md)]
