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
[Link to project](https://github.com/bigelowd-erau/SOLID_E)
This project was made to showcase SOLID design pardigm.

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
