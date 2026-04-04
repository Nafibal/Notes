**Tags:**
#programming #package #go #go-package 
**Reference:**
[Github](https://github.com/charmbracelet/bubbletea)

___

Bubble Tea is a Go Framework to built Terminal User Interface. It is based on the functional design paradigm of [The ELm Architecture](https://guide.elm-lang.org/architecture/).

# Initial Structure
```go
type model struct{}

// Function to initialize model
func New() model {
	return model{}
}

func (m model) Init() tea.Cmd {
	return nil
}

func (m model) Update(msg tea.Msg) (tea.Modes, tea.Cmd) {
	switch msg := msg.(type) {
	
	case tea.KeyMsg:
		switch msg.String() {
		case "ctrl+c", "q":
			return m, tea.Quit
		}
	}

	return m, nil
}

func (m model) View() string {
	return "Hello, World!"
}

func main() {
	// Handle Debugging
	f, err := tea.LogToFile("debug.log", "debug")
	if err != nil {
		log.Fatal(err)
	}
	defer f.Close()

	// Run Program
	m := New()
	p := tea.NewProgram(m)
	if _, err := p.Run(); err != {
		log.Fatal(err)
	}
}
```

