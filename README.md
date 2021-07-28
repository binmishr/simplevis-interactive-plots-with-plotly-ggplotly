# simplevis-interactive-plots-with-plotly-ggplotly

library(simplevis)
library(dplyr)
library(palmerpenguins)

Introduction

simplevis provides gglot2 (and leaflet) wrapper functions to make it easier to make beautiful visualisation with less brainpower required.

In the current post, we discus how to make yoursimplevis ggplot2 plots interactive html widgets using plotly::ggplotly, including how to fine-tune your tooltips.

Make sure you have the latest version of simplevis from CRAN, so that everything runs smoothly (and also that you can access the gg_density* family, which has just been released).
Turning a ggplot into an interactive html widget

The plotly::ggplotly function provides the ability to convert the ggplot object to an interactive plotly html object.

Given that simplevis functions are generally ggplot2 wrapper functions that output ggplot2 objects, we can use this plotly::ggplotly function on our output object to make it interactive.

This is pretty magical, as it is just so easy – thanks plotly!

plot <- gg_point_col(penguins, 
                     x_var = bill_length_mm, 
                     y_var = body_mass_g, 
                     col_var = species, 
                     font_family = "Helvetica")

plotly::ggplotly(plot) 

Turning off widgets other than the camera

The plotly widgets can sometimes look a bit cluttered, especially you are not using them.

Therefore, simplevis provides a plotly_camera function to turn off all widgets other than the camera.

plot <- gg_point_col(penguins, 
                     x_var = bill_length_mm, 
                     y_var = body_mass_g, 
                     col_var = species, 
                     font_family = "Helvetica")

plotly::ggplotly(plot) %>% 
  plotly_camera()

Controlling the tooltip

Some users may want to have more or different variables in the tooltip than plotly::ggplotly defaults provide.

plotly::ggplotly provides a method for this which simplevis uses.

In the simplevis gg_* function, users can add a variable can be added to the text_var that can be used as the tooltip (for functions other than gg_density*() and gg_boxplot*()).

This variable is then used in the ggplotly tooltip when tooltip = text is added to the ggplotly function.

simplevis provides a mutate_text function which makes it easy to create a column that is a string of variable names and associated values separated by breaks for the tooltip.

This function creates a new column called text that can then be added to the text_var in the gg*() function.

If no column names are provided in a vector to mutate_text, then it defaults to providing all variable names and associated values in the dataset.

plot_data <- penguins %>% 
  mutate_text()

plot <- gg_point_col(plot_data, 
                     x_var = bill_length_mm, 
                     y_var = body_mass_g, 
                     col_var = species, 
                     text_var = text, 
                     font_family = "Helvetica")

plotly::ggplotly(plot, tooltip = "text") %>% 
  plotly_camera()
