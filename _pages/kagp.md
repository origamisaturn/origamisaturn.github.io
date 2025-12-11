---
layout: default
---

![](/assets/images/kagp/kagp_run2_img1_crop1.png)

# KAGP - Kerbal Ascent Guidance Procedure

KAGP is an ascent guidance program written in Python, based on [this paper](https://arc.aiaa.org/doi/10.2514/6.1964-638). It runs from the command line, and connects to Kerbal Space Program via a [kRPC](https://github.com/krpc/krpc) server hosted in the game.


Links:
- [GitHub page](https://github.com/origamisaturn/KAGP)
- [Online documentation](https://origamisaturn.github.io/KAGP)

This is a project I started in September 2023. I have worked on and off on it since then, but a majority of the work was done from the months of May to August in 2024. 

It is intended to be used for Kerbal Space Program with the Realism Overhaul mod, which uses more realistic parameters for spacecraft. KAGP interacts with Kerbal Space Program through a kRPC server, as it exposes the game's API to Python, which is the language I am most comfortable with. KAGP only works for single-stage spacecraft and cannot stage vessels in-game. While implementing a multistage algorithm would be generally more useful and allow for launches from Earth, I chose to implement a single-stage ascent algorithm so that I would have an easier time learning how the math for it works.

Initially, I intended to keep the program barebones and bring it to a point where it *just* functioned, but after working on it for a while I was adding features beyond the guidance algorithm. I wanted practice in polishing a program, and I would do it for as long as I was motivated to. Here are some of its features:

### Command Line Interface

<figure style="float: right; width: 50%; height: auto; margin-left: 20px;">
    <img src="/assets/images/kagp/kagp_sample_config.png" width=778 height=707
        style="height:auto;"/>
    <figcaption>
        An example KAGP configuration file.
    </figcaption>
</figure>

The user interacts with KAGP through its command line interface, installed using pip. To run the ascent guidance, the user provides a [configuration file](https://origamisaturn.github.io/KAGP/inputs/) that contains spacecraft info, the intended orbit, and other parameters. An example invocation of KAGP is

```
kagp run example_config.yaml
```

There are plenty of configuration options, so I used Pydantic to parse and validate the configuration files. It has the advantage of being readable and making configuration files easily extendable. There are two subcommands: one for running the ascent guidance, and the other for plotting log files, mainly for debugging.

### Test Suite

Early on in development, I was having plenty of trouble getting the algorithm to work reliably. That led me to set up [test cases](https://github.com/origamisaturn/KAGP/tree/main/kagp/tests), the most extensive of which was for the components of the guidance algorithm. I set up GitHub Actions to automatically run the tests whenever I create a pull request, and to update badges in the README.md that report the status of the test. Creating the tests was time-consuming, but development became much easier. It was especially useful when I made major changes to the program structure, as I could quickly verify whether or not I had broken something.

### Documentation

Throughout the project I used [Google style docstrings](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods) for functions and classes. Some parts, like the implementation of the algorithm or the interconnections between different guidance components, I had difficulty explaining in the code. For these parts, I set up a documentation site hosted with GitHub Pages. I would have preferred to keep the documentation exclusively as markdown files within the GitHub project itself, since GitHub renders markdown pages. However, I ran into trouble with displaying math when I used anything more than basic LaTeX math commands, and I could not get consistent formatting across devices. With a website, I can use [MathJax](https://www.mathjax.org/) to render LaTeX-style math, and I can better support mobile devices.

<figure>
    <video width=1280 height=720 style="width: 100%; height: auto;" autoplay muted loop>
        <source src="/assets/videos/kagp/kagp_run2_cut1.webm">
    </video>
    <figcaption>
        A launch from the lunar surface with KAGP guidance.
    </figcaption>
</figure>

## Conclusion

KAGP can fairly accurately reach the orbital parameters that it is targeting. Here is an example of the final orbital elements for a launch from the Moon using KAGP:

| Orbital Elements                  | Target    | Actual    |
| ---                               | ---       | ---       |
| apoapsis (km)                     | 1790.000  | 1790.764  |
| periapsis (km)                    | 1780.000  | 1780.033  |
| longitude of ascending node (deg) | 171.887   | 171.874   |
| inclination (deg)                 | 2.000     | 2.000     |
| argument of periapsis (deg)       | 17.189    | 18.967    |

The program can accurately reach the orbital plane it is targeting (longitude of ascending node and inclination within 0.01 degrees of target value) but has more trouble with the shape of the orbit within that plane. The error mostly comes from relying on full throttle or no throttle commands, and the commanded throttle cutoff time occurring between game physics timesteps: I cannot get decrease the error appreciably without reducing thrust near main engine cutoff, which I have not implemented.

KAGP works well when it works, but the algorithm can also fail to converge and the program will throw an exception. This is normal, but it would be helpful if the user received more information to determine why it failed, and features to make it occur less often. From my testing, the most likely place for the algorithm to encounter an error is the [`VThetaSolver` component](https://github.com/origamisaturn/KAGP/blob/79b26f2a60db64cd07e9b627abe772f7176e943f/kagp/guidance_components.py#L610). When estimating the final velocity of an ascent trajectory, sometimes `VThetaSolver` will encounter a time when the demanded acceleration from the spacecraft is more than that spacecraft can provide, and it leads to an exception being thrown. This can be caused by the targeted orbital elements demanding an impractical ascent trajectory. 

Diagnosing issues with the ascent can be improved with the addition of some plots. For example, one plot could be a 2D projection of the planet being launched from, with the ground track of the target orbit and the point on the surface where the spacecraft is being launched from. This plot can help the user determine if the launch position occurs far from the target orbital plane, which leads to excessive yaw commands. Another plot could be a side view of the intended ascent trajectory, with arrows indicating the radial and prograde components of the acceleration at different times during the ascent. This would indicate whether or not the ascent trajectory has excessive radial thrust commands, leaving insufficient thrust for prograde acceleration. In addition, the issue with excessive yaw commands can be alleviated if a setting was provided to set the launch to occur only when the launch position is a certain distance from the target orbital plane.

Overall, I am satisfied with how the KAGP turned out, and it was good practice in structuring a program and maintaining motivation for a project.
