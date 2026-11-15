
import React, { useEffect, useState } from "react";
import {
  Sparkles, Rocket, Code2, Palette, Database,
  Smartphone, Languages, ShieldCheck, CheckCircle2,
  ChevronRight, ChevronLeft, ListTodo, Image,
  Github, Globe
} from "lucide-react";

const phases = [
  {
    title: "Project Setup",
    icon: Github,
    description: "Prepare the Imagine AI project and GitHub repository.",
    advice: "Start with a clean React project. Keep your code organized and save every working version to GitHub.",
    stack: ["GitHub", "React", "Vite", "JavaScript"],
    tasks: [
      "Create the Imagine AI GitHub repository",
      "Set up the React project",
      "Create the main App.jsx file",
      "Add the Imagine AI logo and app name"
    ]
  },
  {
    title: "App Design",
    icon: Palette,
    description: "Build a beautiful and simple AI image generator interface.",
    advice: "Use a mobile-first design. Make image generation easy even for first-time users.",
    stack: ["React", "CSS", "Lucide Icons"],
    tasks: [
      "Create the home screen",
      "Add prompt input box",
      "Add Generate Image button",
      "Design image preview section",
      "Make the interface responsive"
    ]
  },
  {
    title: "AI Integration",
    icon: Sparkles,
    description: "Connect Imagine AI to an image-generation service.",
    advice: "Use an image-generation API through a secure backend. Never put secret API keys inside frontend React code.",
    stack: ["Image Generation API", "Backend", "REST API"],
    tasks: [
      "Choose an image-generation provider",
      "Create a secure backend endpoint",
      "Connect the frontend to the endpoint",
      "Send image prompts to the AI",
      "Display generated images"
    ]
  },
  {
    title: "Language Support",
    icon: Languages,
    description: "Make Imagine AI available in Assamese and English.",
    advice: "Users should be able to write prompts in Assamese or English. Add a simple language selector.",
    stack: ["Assamese", "English", "Unicode"],
    tasks: [
      "Add Assamese language option",
      "Add English language option",
      "Create language selector",
      "Test Assamese text rendering",
      "Test Assamese image prompts"
    ]
  },
  {
    title: "User Features",
    icon: Database,
    description: "Add useful features to make Imagine AI a complete product.",
    advice: "Start with basic features. Add image download, prompt history and loading indicators before advanced features.",
    stack: ["React", "Database", "Cloud Storage"],
    tasks: [
      "Add image download option",
      "Add prompt history",
      "Add loading animation",
      "Add error messages",
      "Add image gallery"
    ]
  },
  {
    title: "Testing & Launch",
    icon: Rocket,
    description: "Test Imagine AI and prepare it for Android users.",
    advice: "Test on your Samsung Android phone. Check mobile responsiveness, image generation, download and API security before release.",
    stack: ["Android", "GitHub", "Web Hosting", "PWA"],
    tasks: [
      "Test on Android phone",
      "Fix interface bugs",
      "Test image generation",
      "Publish the web app",
      "Prepare Android app release"
    ]
  }
];

export default function App() {
  const [active, setActive] = useState(0);
  const [completed, setCompleted] = useState(() => {
    try {
      return JSON.parse(
        localStorage.getItem("imagineAIProgress") || "[]"
      );
    } catch {
      return [];
    }
  });

  useEffect(() => {
    localStorage.setItem(
      "imagineAIProgress",
      JSON.stringify(completed)
    );
  }, [completed]);

  const total = phases.reduce(
    (sum, phase) => sum + phase.tasks.length, 0
  );

  const progress = Math.round(
    (completed.length / total) * 100
  );

  const phase = phases[active];
  const done = phase.tasks.filter(
    (_, i) => completed.includes(`${active}-${i}`)
  ).length;

  function toggle(id) {
    setCompleted(old =>
      old.includes(id)
        ? old.filter(item => item !== id)
        : [...old, id]
    );
  }

  const Icon = phase.icon;

  return (
    <div className="min-h-screen bg-[#080b16] text-white">
      <header className="sticky top-0 z-20 border-b border-white/10 bg-[#080b16]/95 p-4">
        <div className="mx-auto flex max-w-6xl items-center justify-between gap-3">
          <div className="flex items-center gap-3">
            <div className="rounded-xl bg-blue-600 p-2">
              <Sparkles size={24}/>
            </div>
            <div>
              <h1 className="text-lg font-bold">Imagine AI</h1>
              <p className="text-xs text-blue-400">Development Dashboard</p>
            </div>
          </div>
          <div className="text-right">
            <p className="text-xs text-gray-400">Overall progress</p>
            <p className="text-xl font-bold text-blue-400">{progress}%</p>
          </div>
        </div>
        <div className="mx-auto mt-3 h-2 max-w-6xl overflow-hidden rounded-full bg-gray-800">
          <div
            className="h-full rounded-full bg-blue-500 transition-all"
            style={{width: `${progress}%`}}
          />
        </div>
      </header>

      <main className="mx-auto grid max-w-6xl gap-6 p-4 md:grid-cols-[260px_1fr] md:p-8">
        <aside>
          <h2 className="mb-4 text-sm font-semibold uppercase tracking-wider text-gray-400">
            Development Roadmap
          </h2>
          <div className="flex gap-2 overflow-x-auto pb-3 md:flex-col md:overflow-visible">
            {phases.map((p, i) => {
              const StepIcon = p.icon;
              const count = p.tasks.filter(
                (_, j) => completed.includes(`${i}-${j}`)
              ).length;
              return (
                <button
                  key={p.title}
                  onClick={() => setActive(i)}
                  className={`min-w-48 rounded-xl border p-4 text-left transition md:min-w-0 ${
                    active === i
                      ? "border-blue-500 bg-blue-600/15"
                      : "border-gray-800 bg-[#111827]"
                  }`}
                >
                  <div className="flex items-center gap-3">
                    <StepIcon className="text-blue-400" size={22}/>
                    <div>
                      <p className="text-xs text-gray-500">Phase {i+1}</p>
                      <p className="text-sm font-semibold">{p.title}</p>
                    </div>
                  </div>
                  <p className="mt-2 text-xs text-gray-400">
                    {count}/{p.tasks.length} completed
                  </p>
                </button>
              );
            })}
          </div>
        </aside>

        <section className="rounded-2xl border border-gray-800 bg-[#111827] p-5 sm:p-8">
          <div className="mb-6">
            <div className="mb-4 flex items-center gap-2 text-blue-400">
              <Icon size={25}/>
              <span className="text-sm">Phase {active+1} of 6</span>
            </div>
            <h2 className="text-3xl font-bold">{phase.title}</h2>
            <p className="mt-3 text-gray-400">{phase.description}</p>
          </div>

          <div className="mb-7 rounded-xl border border-blue-500/20 bg-blue-500/5 p-5">
            <h3 className="mb-2 flex items-center gap-2 font-semibold">
              <Rocket size={18} className="text-blue-400"/>
              Development Guide
            </h3>
            <p className="text-sm leading-7 text-gray-300">
              {phase.advice}
            </p>
          </div>

          <div className="mb-7">
            <h3 className="mb-3 flex items-center gap-2 font-semibold">
              <Code2 size={19} className="text-blue-400"/>
              Recommended Tools
            </h3>
            <div className="flex flex-wrap gap-2">
              {phase.stack.map(tool => (
                <span key={tool} className="rounded-lg border border-gray-700 bg-gray-800 px-3 py-2 text-sm">
                  {tool}
                </span>
              ))}
            </div>
          </div>

          <div>
            <div className="mb-4 flex items-center justify-between">
              <h3 className="flex items-center gap-2 font-semibold">
                <ListTodo className="text-green-400" size={20}/>
                Action Checklist
              </h3>
              <span className="text-sm text-green-400">
                {done}/{phase.tasks.length}
              </span>
            </div>

            <div className="space-y-3">
              {phase.tasks.map((task, i) => {
                const id = `${active}-${i}`;
                const checked = completed.includes(id);
                return (
                  <label
                    key={id}
                    className="flex cursor-pointer items-center gap-3 rounded-xl border border-gray-800 bg-gray-900/70 p-4"
                  >
                    <input
                      type="checkbox"
                      checked={checked}
                      onChange={() => toggle(id)}
                      className="h-5 w-5 accent-blue-500"
                    />
                    <span className={`text-sm ${checked ? "text-gray-500 line-through" : "text-gray-200"}`}>
                      {task}
                    </span>
                    {checked && <CheckCircle2 size={18} className="ml-auto shrink-0 text-green-400"/>}
                  </label>
                );
              })}
            </div>
          </div>

          <div className="mt-8 flex justify-between gap-3 border-t border-gray-800 pt-6">
            <button
              onClick={() => setActive(Math.max(0, active-1))}
              disabled={active===0}
              className="flex items-center gap-2 rounded-lg bg-gray-800 px-4 py-3 text-sm disabled:opacity-30"
            >
              <ChevronLeft size={18}/> Previous
            </button>
            <button
              onClick={() => setActive(Math.min(5, active+1))}
              disabled={active===5}
              className="flex items-center gap-2 rounded-lg bg-blue-600 px-4 py-3 text-sm font-semibold disabled:opacity-30"
            >
              Next Phase <ChevronRight size={18}/>
            </button>
          </div>
        </section>
      </main>

      <footer className="border-t border-gray-800 p-5 text-center text-xs text-gray-500">
        <Sparkles size={14} className="mr-1 inline text-blue-400"/>
        Imagine AI © 2026 — Create Beyond Imagination
      </footer>
    </div>
  );
}
# Project-name
