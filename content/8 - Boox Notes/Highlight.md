<%* 
const NOTE_STYLES = {
  "!": { type: "important", title: "Striking/Intense" },
  "@": { type: "danger", title: "In Discord" },
  "?": { type: "question", title: "Thought Provoking" },
  "/": { type: "sceptic", title: "Sceptic" },
  "~": { type: "warning", title: "Unsound" },
  "#": { type: "stylish", title: "Stylish" }
}

const NOTE_STYLE_KEYS = Object.keys(NOTE_STYLES);
const DEFAULT_NOTE_STYLE = { type: "quote", title: "Thoughts" }

function parseNoteFormat(note) {
  let data = {...DEFAULT_NOTE_STYLE, note: note.trim()};
  for (let key of NOTE_STYLE_KEYS) {
    if (note.startsWith(key)) {
      data = {...NOTE_STYLES[key], note: note.substring(key.length).trim()};
      break;
    }
  }
  return data
}

function getTitleAndAuthor(l) {
  let parts = l.split("<<")[1].split(">>");
  return { title: parts[0].trim(), authors: parts[1].trim() }
}

function parseNote(note) {
  let lines = note.split("\n")
    .map(l => l.trim())
    .filter(l => l.length > 0);
  
  lines = lines.reverse();
  let content = {
    section: "",
    timestamp: "",
    page: "",
    highlight: "",
    note: "",
    continued: false
  }
  
  for (let i = 0; i < lines.length; i++) {
    let l = lines[i];
    
    if (l.includes("【Annotation】")) {
      const noteContent = l.replace('【Annotation】', "").trim();
      content.note = parseNoteFormat(noteContent);
    } else if (l.includes("  |  Page No.: ")) {
      let meta = l.split("  |  Page No.: ");
      content.timestamp = window.moment(meta[0].trim()).format("DD MMM YYYY hh:mm:ss A")
      content.page = parseInt(meta[1])
    } else if (i == lines.length - 1) {
      content.section = l.trim();
    } else {
      content.highlight = content.highlight ? `${l} ${content.highlight}` : l;
    }
  }
  
  if (!content.note) {
    content.note = {...DEFAULT_NOTE_STYLE, note: ""}
  }
  
  return content
}

let file = app.workspace.getActiveFile()
const content = await tp.system.prompt("Paste the JSON content", null, true, true);

let lines = content.split("\n");
let titleAndAuthor = getTitleAndAuthor(lines.shift())

const notes = (lines.join("\n") + "\n")
  .split("-------------------\n")
  .map(n => {
    let v = n.trim();
    if (v.length) {
      return parseNote(v);
    }
    return null;
  })
  .filter(n => n !== null);

let output = `# ${titleAndAuthor.title}\n##### ${titleAndAuthor.authors}\n\n`;
let currentSection = null;

for (let i = 0; i < notes.length; i++) {
  let noteData = notes[i]
  
  if (noteData.section && (currentSection != noteData.section)) {
    output += `## ${noteData.section}\n`;
    currentSection = noteData.section;
  }
  
  // 直接输出高亮内容，不再添加页码标题
  output += `${noteData.highlight}\n\n`;
  output += `> [!${noteData.note.type}] ${noteData.note.title}`;
  
  if (noteData.note.note) {
    if (noteData.note.note.length > 50) {
      output += `\n> ${noteData.note.note}`
    } else {
      output += `: ${noteData.note.note}`;
    }
    output += "\n";
  }
  output += "\n\n"
}

await app.vault.modify(file, output)
%>