---
authors:
- admin
date: "2021-06-30T00:00:00Z"
draft: false
featured: false
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/yEauzeZU6xo)'
  focal_point: ""
  placement: 2
  preview_only: true
lastmod: "2021-06-30T00:00:00Z"
projects: []
subtitle: 'Welcome to STA 198 / GLHLTH 298!'
summary: This is a tentative course schedule. The flow of topics might change slightly depending on how quickly / slowly it feels right to progress through them. However the deadlines for assignments will not change barring unexpected circumstances.
title: 'Overview'
---



The figure below is a visual summary of the tentative course schedule. The flow of topics for each week might change slightly depending on how quickly / slowly it feels right to progress through them. However, the deadlines for assignments will not change barring unexpected circumstances, which, unfortunately, may occur this semester! If deadlines need to be adjusted after the first day of class, they will be pushed to a later date and never pulled up to an earlier date. If it becomes necessary to make such adjustments, I will try my best to avoid collision with other previously scheduled deadlines, but this might be difficult to ensure.


```{r echo=FALSE, warning=FALSE, message=FALSE, out.width="100%", fig.asp=0.9}

library(tidyverse)
library(viridis)
library(sugrrants)
library(lubridate)

d <- read_csv("course-calendar.csv") %>%
  mutate(date = dmy(date))

d_cal <- d %>%
  frame_calendar(
    x = 1, y = 1, date = date,
    calendar = "weekly", week_start = -1
  ) %>%
  mutate(
    day_short = str_sub(day, 1, 1),
    day_short = if_else(day == "Thursday", "Th", day_short),
    day_short = if_else(day == "Saturday", "Sa", day_short),
    day_short = if_else(day == "Sunday", "Su", day_short),
    day_of_month = day(date),
    month = month(date, label = TRUE, abbr = TRUE),
    day_of_month = paste(month, day_of_month),
    due_type = fct_relevel(due_type, "Homework", "Quiz", "Lab", "Project", "Feedback survey", "Exam")
  )

d_cal %>%
  ggplot(aes(x = .x, y = .y)) +
  geom_tile(aes(fill = due_type, alpha = mark_type), colour = "gray50") +
  # labels
  geom_text(
    data = d_cal %>% filter(mark_type == "Graded"),
    aes(label = due_detail), color = "white",
    size = 3, fontface = "bold"
  ) +
  geom_text(
    data = d_cal %>% filter(mark_type != "Graded"),
    aes(label = due_detail), color = "white",
    size = 3, fontface = "bold"
  ) +
  # day
  geom_text(
    data = d_cal %>% filter(is.na(due_type)),
    aes(label = day_short), color = "grey50", size = 2,
    nudge_x = -0.08, nudge_y = 0.02, hjust = 0
  ) +
  geom_text(
    data = d_cal %>% filter(!is.na(due_type)),
    aes(label = day_short), color = "white", size = 2,
    nudge_x = -0.08, nudge_y = 0.02, hjust = 0
  ) +
  # date
  geom_text(
    data = d_cal %>% filter(is.na(due_type)),
    aes(label = day_of_month), color = "grey50", size = 2,
    nudge_x = 0.08, nudge_y = -0.01, hjust = 1
  ) +
  geom_text(
    data = d_cal %>% filter(!is.na(due_type)),
    aes(label = day_of_month), color = "white", size = 2,
    nudge_x = 0.08, nudge_y = -0.01, hjust = 1
  ) +
  # colors
  scale_fill_manual(
    values = c(
      "Homework"         = "#8fada7",
      "Quiz"             = "#8A8A8A",
      "Lab"              = "#99B2DD",
      "Project"          = "#5A5D74",
      "Exam"     = "#e9d968",
      "Feedback survey"  = "#E9AFA3"
    ),
    na.translate = FALSE
  ) +
  # alpha
  scale_alpha_manual(values = c("Graded" = 1, "Not graded" = 0.8, "Exam" = 0.8), guide = "none") +
  # theme
  theme_void() +
  theme(
    legend.position = "bottom",
    plot.caption = element_text(face = "italic", colour = "gray50", hjust = 1, vjust = 33)
  ) +
  labs(
    fill = "",
    caption = ""
  ) +
  geom_text(
    data = d_cal %>% filter(day == "Monday"),
    aes(y = .y, label = paste("Week", week)), x = -0.1,
    hjust = 1, color = "grey50"
  ) +
  coord_cartesian(xlim = c(-0.2, 1.03), clip = "off")
```



