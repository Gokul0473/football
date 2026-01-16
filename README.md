-- 1. Completely delete the table if it exists to start fresh
drop table if exists game_moves;

-- 2. Re-create the table
create table game_moves (
  room_id text primary key,
  striker_move int,
  gk_move int,
  striker_score int default 0,
  gk_score int default 0
);

-- 3. Turn on the "Realtime" toggle manually via SQL
-- This avoids the "publication" error by checking if it exists first
do $$
begin
  if not exists (select 1 from pg_publication where pubname = 'supabase_realtime') then
    create publication supabase_realtime;
  end if;
end $$;

-- 4. Add the table to the realtime list
alter publication supabase_realtime add table game_moves;
