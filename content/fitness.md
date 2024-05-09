+++
title = 'Fitness Helper'
date = 2024-05-09T12:00:03-04:00
draft = false
+++

This is another kind of automation that I have, it is utilizing gomail to send an email to myself. There is a skeleton version of this project on [GitHub](https://github.com/m-r-maxwell/fitness)

The point of this is I have a current schedule of working out Monday, Wednesday, Friday, and Sunday, I still want to do something on Tuesdays, Thursdays, and Saturdays, but maybe a bit more relaxed, so that's why I want to pick 4 different exercises that fall in to a couple of different categories such as full body, core, chest, legs, yoga, etc..


```go
func CreateEmailBody() string {
	wfd := make([]models.Exercise, 4)
	wfds := make([]string, 4)
	// we need to pick 4 random exercises from the list
	seed := time.Now().UnixNano()
	rdr := rand.New(rand.NewSource(seed))
	listLen := len(models.List)
	wfd[0] = models.List[rdr.Intn(listLen)]
	wfd[1] = models.List[rdr.Intn(listLen)]
	wfd[2] = models.List[rdr.Intn(listLen)]
	wfd[3] = models.List[rdr.Intn(listLen)]
	// check to see if we have any duplicates
	for i := 0; i < 4; i++ {
		for j := i + 1; j < 4; j++ {
			if wfd[i].Name == wfd[j].Name {
				wfd[j] = models.List[rdr.Intn(listLen)]
			}
		}
	}
	for x, v := range wfd {
		modText := strings.Join(v.Modifications, ", ")
		wStr := fmt.Sprintf("# of Reps: %d\n", v.Reps)
		if v.WorkingTime > 0 {
			wStr = fmt.Sprintf("Working Time: %d seconds each\n", v.WorkingTime)
		}
		str := fmt.Sprintf("Exercise %d: %s \nSets: %d \n", x+1, v.Name, v.Sets)
		str += wStr
		str += fmt.Sprintf("Description: %s\nPossible Modifications: %v", v.Description, modText)
		wfds[x] = str
	}
	// print out the exercises
	final := "Exercise List\n------------------------------------------------\n"
	for _, v := range wfds {
		final += fmt.Sprintf("%s\n", v)
		final += "------------------------------------------------\n"
	}
	return final
}
// list of exercises
var List = []Exercise{
	PushUps,
	DeclinePushUps,
	Squats,
	Lunges,
	Plank,
	Crunches,
	JumpingJacks,
	MountainClimbers,
	DownwardDog,
	ChildsPose,
	CactusPose,
	BoundAnglePose,
	GluteBridge,
	Superman,
	CatCow,
	BirdDog,
	BackExtension,
	HighKnees,
	StandingSideCrunch,
	StandingAlternatingCrunch,
	StandingToeTouches,
	ThreadTheNeedle,
	ButtKick,
	VSit,
	VUp,
	HollowBodyHold,
	Burpees,
	Situps,
}
```

Like [Irish WOTD](../irish-wotd), this uses cronjobs to automate this binary running. It could be extended further by including the flags pkg and including something like

```go
    var isManual bool
    // this allows flag.Parse to see --manual and set the true/false value to isManual
	flag.BoolVar(&isManual, "manual", false, "Set to true if you want to manually send the email")
	flag.Parse()

	if isManual {
		final := h.CreateEmailBody()
		h.SendEmail(sender, password, final)
		os.Exit(0)
	}
```