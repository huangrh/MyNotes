# Using ggplot in Python - plotnine

[Using ggplot in Python: Visualizing Data With plotnine](https://realpython.com/ggplot-python/)

[plotnine is an implementation of a grammar of graphics in Python, it is based on ggplot2](https://plotnine.readthedocs.io/en/stable/index.html#)

```
pip install plotnine
```

```
from plotnine.data import mpg, huron
from plotnine import ggplot, aes, geom_point, geom_bar, stat_bin

ggplot(mpg) + aes(x="class", y="hwy") + geom_point()

ggplot(mpg) + aes(x="class") + geom_bar()

ggplot(huron) + aes(x="level") + stat_bin(bins=10) + geom_bar()

ggplot(huron) + aes(x="level") + geom_histogram(bins=10)

(
  ggplot(huron)
  + aes(x="factor(decade)", y="level")
  + geom_boxplot()
)

(
    ggplot(economics)
    + aes(x="date", y="pop")
    + scale_x_timedelta(name="Years since 1970")
    + labs(title="Population Evolution", y="Population")
    + geom_line()
)

ggplot(mpg) + aes(x="class") + geom_bar() + coord_flip()

(
    ggplot(mpg)
    + facet_grid(facets="year~class")
    + aes(x="displ", y="hwy")
    + labs(
        x="Engine Size",
        y="Miles per Gallon",
        title="Miles per Gallon for Each Year and Vehicle Class",
    )
    + geom_point()
)

(
    ggplot(mpg)
    + facet_grid(facets="year~class")
    + aes(x="displ", y="hwy")
    + labs(
        x="Engine Size",
        y="Miles per Gallon",
        title="Miles per Gallon for Each Year and Vehicle Class",
    )
    + geom_point()
    + theme_dark()
)

(
    ggplot(mpg)
    + aes(x="cyl", y="hwy", color="class")
    + labs(
        x="Engine Cylinders",
        y="Miles per Gallon",
        color="Vehicle Class",
        title="Miles per Gallon for Engine Cylinders and Vehicle Classes",
    )
    + geom_point()
)

# 
myPlot = ggplot(economics) + aes(x="date", y="pop") + geom_line()
myPlot.save("myplot.png", dpi=600)

# 
(ggplot(mtcars, aes('wt', 'mpg', color='factor(gear)'))
 + geom_point()
 + stat_smooth(method='lm')
 + facet_wrap('~gear'))
```
