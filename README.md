Multihighlight plugin for flot
==============================

Plugin enables mouseover highlighting data points for multiple plot series and even multiple plots (if they have a common axis such as date for example).
You can see this plugin in action [here](https://www.portfolionumbers.com/tools/risk/historical/returns).

Usage
-----

### Quick setup

Reference the jquery.flot.multihighlight.js in your markup.

Set these grid options:

	hoverable: true,
	autoHighlight: false,

When creating a plot, pass an additional multihighlight option like this.

	multihighlight: {
		mode: 'x'
	}

This will enable multihighlighting on all series in the plot on x axis.

### Multiple plots

When you have multiple plots that share one axis you can enable multihighlighting for them in a way that when one plot's data point is highlighted
the point with the same shared axis value on the other plot will be highlighted as well. You can see the how it looks like [here](https://www.portfolionumbers.com/tools/risk/historical/returns).

For this to work you need to pass an additional multihighlight option - linkedPlots - an array of plots that are linked to the current plot. An example of how this can be accomplished:

	// plots is a plot array.
	$.each(plots, function(index, plot){
		// Link the plots for highlighting them at the same time.
		plot.getOptions().multihighlight.linkedPlots = new Array();
		$.each(plots, function(innerIndex, innerPlot){
			if (index != innerIndex) {
				plot.getOptions().multihighlight.linkedPlots.push(innerPlot);
			}
		});
	});

This code can be executed after drawing the plots linking them all together. It is rather manual I know, but that's because flot plugin can deal with only one plot at a time so it has to be done externally.
Until someone comes up with a better solution that is.

### Events

This plugin basically works by firing a couple of events indicating when data points where highlighted and unhighlighted.

* multihighlighted(event, position, items). Fires when the data points are highlighted. Position is the mouse pointer position within a plot and items is an array of data points - { serieIndex, dataIndex } objects.
* unmultihighlighted(event) - when the data points lose hover and are unhighlighted.

When using linked plots these events will fire for each plot separately.

Known issues
------------

This plugin still has problems in Internet Explorer so use it at your own risk. You're welcome to fork it and fix it.
