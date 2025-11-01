# Luffydex__config.yml
// Luffypedia - Single-file React App
// How to use:
// 1) Create a new React app (Vite recommended): `npm create vite@latest luffypedia --template react`
// 2) Replace src/App.jsx with this file's contents, install dependencies: `npm i framer-motion`
// 3) Make sure you have Tailwind set up, or replace Tailwind classes with your CSS.

import React, { useState, useMemo } from 'react';
import { motion } from 'framer-motion';

// --- Sample static dataset for Monkey D. Luffy ---
const LUFFY = {
  name: "Monkey D. Luffy",
  epithet: "Straw Hat Luffy",
  dob: "May 5 (unknown year)",
  bounty: "3,000,000,000 Berries",
  devilFruit: "Gomu Gomu no Mi (Hito Hito no Mi, Model: Nika)",
  affiliation: "Straw Hat Pirates",
  haki: ["Kenbunshoku Haki", "Busoshoku Haki", "Haoshoku Haki"],
  description:
    "A cheerful, freedom-loving pirate who dreams of becoming the Pirate King. Known for his rubber body, boundless determination, and fierce devotion to friends.",
  avatar:
    "https://upload.wikimedia.org/wikipedia/en/2/2f/Monkey_D._Luffy.png",
  gallery: [
    "https://static.wikia.nocookie.net/onepiece/images/8/8b/Luffy_Anime_Post_Timeskip.png",
    "https://static.wikia.nocookie.net/onepiece/images/5/5b/2019_Luffy.png",
    "https://static.wikia.nocookie.net/onepiece/images/0/0b/LuffyGear5.png",
  ],
  timeline: [
    { year: "0", event: "Born in Foosha Village" },
    { year: "7", event: "Met Shanks and ate the Devil Fruit" },
    { year: "17", event: "Began his pirate journey" },
    { year: "2 Years Later", event: "Returned stronger after training" },
    { year: "Current", event: "Sailing towards the final saga" },
  ],
  notableQuotes: [
    "I'm gonna be King of the Pirates!",
    "I have a crew I can count on."
  ],
};

function Header({ onSearch, query }) {
  return (
    <header className="w-full bg-gradient-to-r from-red-500 via-yellow-400 to-red-600 text-white p-6">
      <div className="max-w-6xl mx-auto flex items-center justify-between">
        <div>
          <h1 className="text-3xl font-extrabold tracking-tight">Luffypedia</h1>
          <p className="text-sm opacity-90">All things Monkey D. Luffy — profiles, timeline, gallery, and facts.</p>
        </div>
        <div className="w-80">
          <input
            aria-label="Search Luffypedia"
            value={query}
            onChange={(e) => onSearch(e.target.value)}
            className="w-full rounded-md px-3 py-2 text-black"
            placeholder="Search Luffy — try 'Haki', 'Gear', 'bounty'..."
          />
        </div>
      </div>
    </header>
  );
}

function ProfileCard({ data }) {
  return (
    <motion.section initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }} className="bg-white shadow-md rounded-2xl p-6">
      <div className="flex flex-col md:flex-row items-center gap-6">
        <img src={data.avatar} alt={data.name} className="w-36 h-36 object-cover rounded-full" />
        <div className="flex-1">
          <h2 className="text-2xl font-bold">{data.name} — <span className="text-yellow-600">{data.epithet}</span></h2>
          <p className="mt-2 text-sm text-gray-700">{data.description}</p>

          <div className="mt-4 grid grid-cols-2 gap-2 text-sm">
            <div><span className="font-semibold">Devil Fruit:</span> {data.devilFruit}</div>
            <div><span className="font-semibold">Affiliation:</span> {data.affiliation}</div>
            <div><span className="font-semibold">Bounty:</span> {data.bounty}</div>
            <div><span className="font-semibold">DOB:</span> {data.dob}</div>
          </div>

          <div className="mt-4">
            <h3 className="font-semibold">Haki</h3>
            <div className="flex gap-2 mt-2 flex-wrap">
              {data.haki.map((h) => (
                <span key={h} className="px-3 py-1 rounded-full bg-gray-100 text-sm">{h}</span>
              ))}
            </div>
          </div>
        </div>
      </div>
    </motion.section>
  );
}

function Timeline({ items }) {
  return (
    <div className="bg-white shadow-md rounded-2xl p-6">
      <h3 className="text-lg font-semibold mb-4">Timeline</h3>
      <ol className="relative border-l border-gray-200">
        {items.map((t, i) => (
          <li key={i} className="mb-6 ml-6">
            <span className="absolute -left-3 flex h-6 w-6 items-center justify-center rounded-full bg-red-500 text-white text-xs">{i + 1}</span>
            <time className="mb-1 text-sm font-normal leading-none text-gray-500">{t.year}</time>
            <p className="text-sm text-gray-700">{t.event}</p>
          </li>
        ))}
      </ol>
    </div>
  );
}

function Gallery({ images }) {
  return (
    <div className="bg-white shadow-md rounded-2xl p-6">
      <h3 className="text-lg font-semibold mb-4">Gallery</h3>
      <div className="grid grid-cols-1 sm:grid-cols-3 gap-3">
        {images.map((src, i) => (
          <figure key={i} className="overflow-hidden rounded-lg">
            <img src={src} alt={`Luffy ${i + 1}`} className="w-full h-48 object-cover transform hover:scale-105 transition" />
          </figure>
        ))}
      </div>
    </div>
  );
}

function Facts({ quotes }) {
  return (
    <div className="bg-white shadow-md rounded-2xl p-6">
      <h3 className="text-lg font-semibold mb-4">Notable Quotes</h3>
      <ul className="list-disc pl-5 space-y-2 text-gray-700">
        {quotes.map((q, i) => (
          <li key={i} className="italic">"{q}"</li>
        ))}
      </ul>
    </div>
  );
}

export default function App() {
  const [query, setQuery] = useState('');

  // Simple search across a few fields
  const results = useMemo(() => {
    const q = query.trim().toLowerCase();
    if (!q) return { match: true, data: LUFFY };

    const hay = [
      LUFFY.name,
      LUFFY.epithet,
      LUFFY.devilFruit,
      LUFFY.affiliation,
      LUFFY.bounty,
      LUFFY.haki.join(' '),
      LUFFY.description,
    ].join(' ').toLowerCase();

    return { match: hay.includes(q), data: LUFFY };
  }, [query]);

  return (
    <div className="min-h-screen bg-gradient-to-b from-gray-100 to-gray-50">
      <Header onSearch={setQuery} query={query} />

      <main className="max-w-6xl mx-auto py-10 px-4 space-y-6">
        {!results.match ? (
          <div className="bg-white p-6 rounded-2xl shadow-md text-center">No results for "{query}" — try another term like "Haki" or "Gear".</div>
        ) : (
          <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <div className="lg:col-span-2 space-y-6">
              <ProfileCard data={results.data} />

              <Gallery images={results.data.gallery} />

              <Facts quotes={results.data.notableQuotes} />
            </div>

            <aside className="space-y-6">
              <Timeline items={results.data.timeline} />

              <div className="bg-white shadow-md rounded-2xl p-6">
                <h3 className="text-lg font-semibold">Quick Stats</h3>
                <ul className="mt-3 text-sm text-gray-700 space-y-2">
                  <li><span className="font-semibold">Bounty:</span> {results.data.bounty}</li>
                  <li><span className="font-semibold">Devil Fruit:</span> {results.data.devilFruit}</li>
                  <li><span className="font-semibold">Affiliation:</span> {results.data.affiliation}</li>
                </ul>
              </div>
            </aside>
          </div>
        )}

        <footer className="text-center text-sm text-gray-500 py-6">
          © Luffypedia — fan project. One Piece and characters are property of Eiichiro Oda / Toei Animation.
        </footer>
      </main>
    </div>
  );
}
