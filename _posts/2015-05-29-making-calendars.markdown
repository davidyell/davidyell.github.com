---
layout: post
title: "Making a calendar"
date: 2015-05-29 11:04:12 +0100
comments: true
categories: ['php', 'dates', 'calendar']
---
## How to generate a calendar using PHP?
At work I built a new holiday calendar system to replace the old system which was being closed down. The company needed a way for employees to request holiday and also managers to check booked holidays by team.

[This is what it looks like once it's been styled](http://i.imgur.com/079nh27.png).

## Why have I written my own code?
A few reasons really, primarily because it's a learning experience and secondly because I don't recall being able to find a decent library with did it. No doubt there are a few, if you know any, please leave a comment and I'll update this post, thanks!

## Calendar code
Just a point to note, this is written in a [CakePHP](http://cakephp.org/) view, and uses a table.

```php
<table summary="calendar" class="table table-bordered" id="holiday-calendar">
	<thead>
		<tr>
			<th><?= __('Monday')?></th>
			<th><?= __('Tuesday')?></th>
			<th><?= __('Wednesday')?></th>
			<th><?= __('Thursday')?></th>
			<th><?= __('Friday')?></th>
			<th><?= __('Saturday')?></th>
			<th><?= __('Sunday')?></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<?php
			// $days is a CakePHP2 data array of days
			
			// Work out when the month starts in the grid
			for ($i = 1; $i <= 7; $i++) {
				if ((int)date('N', strtotime($days[0]['Day']['date'])) === $i) {
					// This is a CakePHP element to make a table cell for the day
					echo $this->element('calendar-day', ['day' => $days[0]]);
					break;
				} else {
					echo "<td>&nbsp;</td>";
				}
			}

			$numOutput = 1;
			$remainingCells = 7 - $i;

			// Output the remaining cells after the first day
			for ($i = 1; $i <= $remainingCells; $i++) {
				echo $this->element('calendar-day', ['day' => $days[$i]]);
				$numOutput++;
			}
			?>
		</tr>

		<tr>
			<?php
			$dayOfWeek = 0;
			// Output the remaining rows
			for ($i = $numOutput; $i < count($days); $i++) {
				echo $this->element('calendar-day', ['day' => $days[$i]]);

				// Look for the end of the week to roll a new row
				$dayOfWeek++;
				if ($dayOfWeek === 7) {
					$dayOfWeek = 0;
					echo "</tr><tr>";
				}
			}

			// Finish off the table with empty rows
			if ($dayOfWeek != 7) {
				for ($i = $dayOfWeek; $i < 7; $i++) {
					echo "<td>&nbsp;</td>";
				}
			}
			?>
		</tr>

	</tbody>
</table>
```

### All done
That's it, you now have a nice horizontal calendar similar to Google Calendar!