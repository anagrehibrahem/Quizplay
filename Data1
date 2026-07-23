import { Category } from './gameData';

// ── Legacy global key (kept for read-only backward compat — never written anymore) ──
const GLOBAL_KEY = 'cat_overrides_v3';

export interface CatOverrideData {
  deleted:  string[];                     // IDs of hidden default categories
  added:    Record<number, Category[]>;   // New categories per round (0-3)
  modified: Record<string, Category>;     // Full overrides for default categories
}

const EMPTY_DATA: CatOverrideData = { deleted: [], added: {}, modified: {} };

// ── Per-user key ───────────────────────────────────────────────────────────────
export function getUserOverridesKey(userId: string) {
  return `cat_overrides_user_${userId}`;
}

export function loadUserCatOverrides(userId: string): CatOverrideData {
  try {
    const raw = localStorage.getItem(getUserOverridesKey(userId));
    if (raw) return JSON.parse(raw) as CatOverrideData;
  } catch {}
  return { deleted: [], added: {}, modified: {} };
}

export function saveUserCatOverrides(userId: string, data: CatOverrideData) {
  localStorage.setItem(getUserOverridesKey(userId), JSON.stringify(data));
}

// ── Legacy global helpers (kept for backward compat only) ─────────────────────
export function loadCatOverrides(): CatOverrideData {
  try {
    const raw = localStorage.getItem(GLOBAL_KEY);
    if (raw) return JSON.parse(raw) as CatOverrideData;
  } catch {}
  return { ...EMPTY_DATA };
}

export function saveCatOverrides(data: CatOverrideData) {
  localStorage.setItem(GLOBAL_KEY, JSON.stringify(data));
}

/** Returns all user IDs that have stored question overrides */
export function getAllUsersWithOverrides(): string[] {
  const keys: string[] = [];
  const prefix = 'cat_overrides_user_';
  for (let i = 0; i < localStorage.length; i++) {
    const key = localStorage.key(i);
    if (key && key.startsWith(prefix)) {
      keys.push(key.replace(prefix, ''));
    }
  }
  return keys;
}

/**
 * Returns round categories with ONLY the current user's overrides applied.
 * Every user has their own isolated copy of deletions / modifications / additions.
 * If no userId is provided, falls back to the legacy global key (guest mode).
 */
export function applyRoundOverrides(
  roundIdx: number,
  defaults: Category[],
  userId?: string,
): Category[] {
  // Load the correct data store
  const data: CatOverrideData = userId
    ? loadUserCatOverrides(userId)
    : loadCatOverrides();

  const base: Category[] = [];
  for (const cat of defaults) {
    if (data.deleted.includes(cat.id)) continue;
    base.push(data.modified[cat.id] ?? cat);
  }

  const added = data.added[roundIdx] ?? [];
  return [...base, ...added];
}

export function isCustomCategory(id: string) { return id.startsWith('cust-'); }

export function generateCatId() {
  return `cust-${Date.now()}-${Math.floor(Math.random() * 10000)}`;
}
export function generateQId(catId: string, pts: number) { return `${catId}-q${pts}`; }

// ── Image presets ──────────────────────────────────────────────────────────────
export const IMAGE_PRESETS = [
  'https://images.unsplash.com/photo-1606326608690-4e0281b1e588?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1726615340002-7dfa5cead37d?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1759868412872-fac3084d37b1?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1488748809185-53ad203ca5cf?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1716879655079-944c96c7cd01?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1729625466342-558a6dfcf670?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1660214248749-8932ef58a8a5?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1720701575003-51dafcf39cb4?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1770320742270-0cacc28b56b9?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1767128312636-de243003b0fe?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1720421502211-eb2901acbf48?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1771607027595-d27c1967f222?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1663658089767-36899e2a897d?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1758721017400-26f4f9397414?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1759272840538-ae4b07214c71?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
  'https://images.unsplash.com/photo-1769144256181-698b8f807066?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&w=400',
];

// Backwards compat
export const DEFAULT_CAT_IMAGES = IMAGE_PRESETS.slice(0, 5);
