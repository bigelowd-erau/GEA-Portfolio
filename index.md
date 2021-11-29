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
"There should never be more than one reason for a class to change."
"A class should have one and only one reason to change, meaning that a class should have only one job."
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

