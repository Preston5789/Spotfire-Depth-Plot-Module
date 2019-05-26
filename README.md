# Spotfire-Depth-Plot-Module
A module for Spotfire that creates depths plots for reservoir logging analysis. The script is written in R and uses Spotfire's TERR function to create programable depth plots within Spotfire.

Spotfire does not allow for area plots, nor does it allow for flipped coordinate axises. This module was created so that the TCMR and BFV porsities can be plotted as a function of depth. The user can also select data points from other graphs in the file and the points will show on the depth plot creating an interactive experience. 

<p align="center">
  <img src="https://github.com/Preston5789/Spotfire-Depth-Plot-Module/blob/master/Depth_Plot_1.PNG" width="350" title="hover text">
  <img src="https://github.com/Preston5789/Spotfire-Depth-Plot-Module/blob/master/Depth_Plot_2.PNG" width="350" title="hover text">
</p>

DepthPlot.dxp contains an example project using the depth plot module.

DepthPlot.sfd contains the Spotfire data function for importing the module into your own project.

Below is the source script:

### The R Script
Setting up libraries and supressing annoying waringings
```
 library(RinR)
library(Sdatasets)
suppressWarnings(suppressPackageStartupMessages(library(ggplot2)))
#
# Path to Open Source R
#
PathToOpenSourceR <- "C:/Program Files/R/R-3.6.0/bin/x64/R"
options( RinR_R_FULL_PATH = PathToOpenSourceR )
#
#
```
The graphing:
```
#  Start of Graphics
#
#
graph <-RGraph(
print(
	ggplot()
		+ geom_area(data = input, aes(y = input$TCMR, x = input$DEPTH, fill = "FFI"), color="black", show.legend =TRUE)
		+ geom_area(data = input, aes(y = input$BFV, x = input$DEPTH,fill = "BFV"), color="black", alpha=0.98,show.legend =TRUE) 
		+ scale_fill_manual(values=c("lightgrey", "yellow"), name="")
		+ labs(y= "CMR Porosity") 
		+ theme_dark()
		+ theme(
			panel.grid.major =   element_line(colour = "white",size=0.01,linetype="dotted"),
			panel.grid.minor =   element_blank(),
			axis.text=element_text(size=12,colour="white"),
			axis.title=element_text(size=16, colour="white"),
			legend.position=c(0.5,0.975),legend.direction="horizontal",
			legend.text = element_text(colour="white", size = 12),
			legend.background = element_rect(fill="gray17", linetype="dotted"),
			panel.background = element_rect(fill="gray17", linetype="dotted"),
			plot.background = element_rect(fill = "gray17",color = NA))
		+ scale_y_reverse()
		+ geom_point(data = markinput, size=4, shape=21,color = "red",fill="white", alpha = 0.75,stroke = 3, aes(x=markinput$DEPTH, y=markinput$TCMR))
		+ ylim(0.5, 0)
		+ coord_flip()
		+ scale_x_continuous("Depth", breaks = seq( max(input$DEPTH), min(input$DEPTH), by = -50),trans = scales::reciprocal_trans())
		), 
		packages = 'ggplot2',
		width = 220, height = 1200
) 
 ```
## Built With

* [ggplot2](https://ggplot2.tidyverse.org/) - Packages for plotting



## Authors

* **Preston Phillips** - [Preston5789](https://github.com/Preston5789)
