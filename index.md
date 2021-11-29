## Doyle DelRay Bigelow

You can use the [editor on GitHub](https://github.com/bigelowd-erau/GEA-Portfolio/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

## SOLID Design Principles
[This project](https://github.com/bigelowd-erau/SOLID_E) was made to showcase SOLID design principles, and based on [Infallible Code's Tutorial](https://www.youtube.com/watch?v=QDldZWvNK_E).

The [playable version](https://bigelowd-erau.github.io/SOLID_E/) is available through github pages.

These principles establish practices that lend to developing software with considerations for maintaining and extending as the project grows. Adopting these practices can also contribute to avoiding code smells, refactoring code, and Agile or Adaptive software development.

SOLID stands for:

    S - Single-responsiblity Principle
    O - Open-closed Principle
    L - Liskov Substitution Principle
    I - Interface Segregation Principle
    D - Dependency Inversion Principle
    
### S: Single Responsibility
**"A class should have one and only one reason to change, meaning that a class should have only one job."**

""_This principle encourages complicated classes to be divided into smaller classes that have explicit responsibilities. While this may seem like a fairly straight forward principle to follow, it is often difficult to put into practice if a class’s responsibility isn’t immediately clear. Martin has helped us capture the responsibility of a class by arguing that responsibility is a “reason to change.” Thus, you can identify bad design when there are multiple entities that change for different reasons._""

The [MouseScreenRayProvider](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/RayProviders/MouseScreenRayProvider.cs) script only handles creating a ray from the center of the viewport.

It does not handle how to cast the ray or what to do with the ray once its cast. It doesn't handle other ways of creating rays.

The only reason this class will ever need to change is if the way providing a rayfrom the center of the screen needs to change.
```markdown
//Creates 1 ray pointing from center of the viewport in the direction of the viewport
public class MouseScreenRayProvider : MonoBehaviour, IRayProvider
{
    public Stack<Ray> CreateRay()
    {
        Stack<Ray> ray = new Stack<Ray>();
        ray.Push(Camera.main.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0)));
        return ray;
    }
}
```
### O: Open/Closed Principle
**"Objects or entities should be open for extension but closed for modification."**

"_Simply put, this principle requires that modules should be written so that they can be extended, without requiring them to be modified. This seems contradictory at first, but the key to making this work is by adequately using abstraction techniques. Proper abstractions allow for features to be added by adding new code and not changing the original codebase. You aren’t likely to break working code if you don’t have to change it._""

The [Switcher](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/Switchers/Switcher.cs) class enables a changeable object to go to the next state based on a keycode input.

Though the issue it would be good to assign a specific keycode based on what type of changeable object is being switched. Instead of dumping the logic into the Switcher class there are extensions to the switcher class that sets the keycode that triggers the next function for each type of switcher that will be needed: such as the [RayProviderSwitcher](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/Switchers/RayProviderSwitcher.cs).
```markdown
//enables switching to next of a changeable type
public abstract class Switcher : MonoBehaviour
{
    //both are provided by the concrete child implementation
    internal IChangeable changeableObject;
    internal KeyCode nextKeyCode;

    private void Update()
    {
        if (Input.GetKeyDown(nextKeyCode))
        {
            changeableObject.Next();
        }
    }
}
```
```markdown
//enables switching to next of a ray provider type
public class RayProviderSwitcher : Switcher
{
    private void Start()
    {
        nextKeyCode = KeyCode.Alpha2;
        changeableObject = GetComponent<IChangeable>();
    }
}
```
### Liskov's Substitution Principle
**“Subclasses should be substitutable for their base classes.”**

"_Originally devised by Barbara Liskov, this principle can be a bit difficult to understand. At its core, it means that “subclasses should add to a base class’s behavior, not replace it.” Ideally, parent instances should be able to replace their child instances without creating any unexpected or mysterious behavior._"

This tutorial project focused more on Interface Segregation than Liskov's Substitution Principle. As such there are no parent child classes where the child adds to the parent behavior but does not change it. In retrospect the Switcher class and child implementations violate this prinicple. Something that would be greate for a refactor to bring the project more inline with SOLID principles.
### Interface Segregation Principle
**“Many client-specific interfaces are better than one general-purpose interface.”**

**"A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use."**

"_It is better to break down main classes into smaller more specific classes rather than try to maintain one large generalized class. This way, the class’ clients aren’t relying on methods that they don’t use._"

This project relied heavily on this principle as everything was based on interfaces and passing them to classes instead of a concrete class.

This project had four interfaces that worked in conjunction: [IChangeable](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/Interfaces/IChangeable.cs), [IRayProvider](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/Interfaces/IRayProvider.cs), [ISelectionResponse](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/Interfaces/ISelectionResponse.cs), and [ISelector](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/Interfaces/ISelector.cs).

The IChangeable was responsible for enabling switching between the concrete class that would be used per a specific type.

The Composite classes were the use for this where it also used one of the other interfaces, i.e. the IRayProvider, and enabled the swithing between what Ray Provider was currently being used. Due to Interface Segregation the [Composite Ray Provider](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/RayProviders/CompositeRayProvider.cs) was able to be subsituted as a RayProvider and delegate the functionality down to another Ray Provider based on which it had selected.
```markdown
public interface IRayProvider
{
    Stack<Ray> CreateRay();
}
```
```markdown
//Enables the switching between different Ray Providers
public class CompositeRayProvider : MonoBehaviour, IRayProvider, IChangeable
{
    private List<IRayProvider> rayProviders;
    int _currentIndex = 0;
    Text label;
    public void Start()
    {
        //the possible ray providers are stored in a child GameObject called "RayProviderHolder"
        rayProviders = transform.GetComponentsInChildren<IRayProvider>().ToList();
        //Remove first element since it will be this CompositeRayProvider
        rayProviders.RemoveAt(0);

        label = GameObject.FindGameObjectWithTag("RayProviderLabel").GetComponent<Text>();
        //Update UI label on awake
        label.text = rayProviders[_currentIndex].GetType().ToString();
    }

    public Stack<Ray> CreateRay()
    {
        if (HasSelection())
            return rayProviders[_currentIndex].CreateRay();
        return null;
    }
    public void Next()
    {
        _currentIndex = (_currentIndex + 1) % rayProviders.Count;
        //Update UI
        label.text = rayProviders[_currentIndex].GetType().ToString();
    }
    private bool HasSelection()
    {
        return _currentIndex > -1 && _currentIndex < rayProviders.Count;
    }
}
```
```markdown
//Creates 1 ray pointing from center of the viewport in the direction of the viewport
public class MouseScreenRayProvider : MonoBehaviour, IRayProvider
{
    public Stack<Ray> CreateRay()
    {
        Stack<Ray> ray = new Stack<Ray>();
        ray.Push(Camera.main.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0)));
        return ray;
    }
}
```
```markdown
//Creates samplesize rays in a circle orbiting around the center of the viewport in the direction of the viewport
public class CircleMouseScreenRayProvider : MonoBehaviour, IRayProvider
{
    private float curAngle = 0.0f;
    private int sampleSize = 100;
    private float dAngle;
    private float radius = 0.02f;

    private void Awake()
    {
        dAngle = 360 / sampleSize;
    }

    public Stack<Ray> CreateRay()
    {
        Stack<Ray> rays = new Stack<Ray>();
        for (int i = 0; i < sampleSize; ++i)
        {
            rays.Push(Camera.main.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0) + new Vector3(Mathf.Sin(curAngle), Mathf.Cos(curAngle), 0) * radius));
            curAngle += dAngle;
        }
        curAngle = 0;
        return rays;
    }
}
```
### Dependency Inversion principle
**"“Depend upon Abstractions. Do not depend upon concretions.”"**

"_This principle suggests that higher-level modules should not depend on lower-level modules. The majority of dependencies should be on abstractions from both the higher-level and lower-level modules as well as details._"

This goes right along with the interface segregation principle where any place a certain type of class was required the code only ever requested the interface that includes the functionality it needs. This is seen across the program includeing in the previous Composite Ray Provider class where it maintained a list of Ray Provider Interfaces, but did not know anything about the concrete  implementation.

Another example in the code is  thte [SelectionManager](https://github.com/bigelowd-erau/SOLID_E/blob/main/SOLID%20Project/Assets/_Project/Scripts/SelectionManager.cs) where upon startup it got components it needed by looking for an interface type and getting the concrete it found that implemented that interface. It then would on update call the generic functions provided by those interfaces and have the concrete class it found handle the functionality. The key note is that selsection manager knows nothing about the concrete class it retrieved only the abstract functions it can call to that class through the interface.
```markdown
public class SelectionManager : MonoBehaviour
{
    private ISelectionResponse _selectionResponse;
    private IRayProvider _rayProvider;
    private ISelector _selector;
    private Transform _currentSelection;

    private void Awake()
    {
        //get first child's selection response this will be the composite
        _selectionResponse = GetComponentsInChildren<ISelectionResponse>()[0];
        //get first child's ray provider this will be the composite
        _rayProvider = GetComponentsInChildren<IRayProvider>()[0];
        //get first child's selector this will be the composite
        _selector = GetComponentsInChildren<ISelector>()[0];
    }

    private void Update()
    {
        if (_currentSelection != null) _selectionResponse.OnDeselect(_currentSelection);

        _selector.Check(_rayProvider.CreateRay());

        _currentSelection = _selector.GetSelection();

        if (_currentSelection != null) _selectionResponse.OnSelect(_currentSelection);
    }   
}
```
# Replay
[This production assignment](https://github.com/bigelowd-erau/Replay) was a display of the command log by recording the input commands of the user and then re-calling those commands in a replay of the scene.
