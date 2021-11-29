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
## Liskov's Substitution Principle
**“Subclasses should be substitutable for their base classes.”**

"_Originally devised by Barbara Liskov, this principle can be a bit difficult to understand. At its core, it means that “subclasses should add to a base class’s behavior, not replace it.” Ideally, parent instances should be able to replace their child instances without creating any unexpected or mysterious behavior._"

