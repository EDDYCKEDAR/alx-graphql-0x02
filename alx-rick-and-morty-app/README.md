Task 4: Query the GraphQL Endpoint
Objective

In this task, you will learn how to query the Rick and Morty GraphQL API to retrieve episode data using Apollo Client in a React-based application. You will:

Set up an interface to type the episode data.

Create a page to display a list of episodes with pagination.

Build a reusable EpisodeCard component to display each episode's details.

Steps to Complete the Task
1. Duplicate the Project

First, duplicate the alx-graphql-0x01 project to alx-graphql-0x02.

cp -r alx-graphql-0x01 alx-graphql-0x02
cd alx-graphql-0x02

2. Create interfaces Directory and Define Types

Create a new directory called interfaces in the root of your project:

mkdir interfaces


Inside the interfaces directory, create a file called index.ts:

touch interfaces/index.ts


Add the following content to index.ts to define the types for episode data and pagination:

interface InfoProps {
  pages: number;
  next: number;
  prev: number;
  count: number;
}

export interface EpisodeProps {
  id: number;
  name: string;
  air_date: string;
  episode: string;
  info: InfoProps;
}

export type EpisodeCardProps = Pick<EpisodeProps, "id" | "name" | "air_date" | "episode">;

3. Modify index.tsx to Query Episodes

Open or create the index.tsx file in the pages directory.

touch pages/index.tsx


Replace the content of index.tsx with the following to display the episode list with pagination:

import { useQuery } from "@apollo/client";
import { GET_EPISODES } from "@/graphql/queries";
import { EpisodeProps } from "@/interfaces";
import EpisodeCard from "@/components/common/EpisodeCard";
import { useEffect, useState } from "react";

const Home: React.FC = () => {
  const [page, setPage] = useState<number>(1);
  const { loading, error, data, refetch } = useQuery(GET_EPISODES, {
    variables: {
      page: page,
    },
  });

  useEffect(() => {
    refetch();
  }, [page, refetch]);

  if (loading) return <h1>Loading...</h1>;
  if (error) return <h1>Error</h1>;

  const results = data?.episodes.results;
  const info = data?.episodes.info;

  return (
    <div className="min-h-screen flex flex-col bg-gradient-to-b from-[#A3D5E0] to-[#F4F4F4] text-gray-800">
      {/* Header */}
      <header className="bg-[#4CA1AF] text-white py-6 text-center shadow-md">
        <h1 className="text-4xl font-bold tracking-wide">Rick and Morty Episodes</h1>
        <p className="mt-2 text-lg italic">Explore the multiverse of adventures!</p>
      </header>

      {/* Main Content */}
      <main className="flex-grow p-6">
        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
          {results &&
            results.map(({ id, name, air_date, episode }: EpisodeProps, key: number) => (
              <EpisodeCard
                id={id}
                name={name}
                air_date={air_date}
                episode={episode}
                key={key}
              />
            ))}
        </div>

        {/* Pagination Buttons */}
        <div className="flex justify-between mt-6">
          <button
            onClick={() => setPage((prev) => (prev > 1 ? prev - 1 : 1))}
            className="bg-[#45B69C] text-white font-semibold py-2 px-6 rounded-lg shadow-lg hover:bg-[#3D9B80] transition duration-200 transform hover:scale-105"
          >
            Previous
          </button>
          <button
            onClick={() => setPage((prev) => (prev < info.pages ? prev + 1 : prev))}
            className="bg-[#45B69C] text-white font-semibold py-2 px-6 rounded-lg shadow-lg hover:bg-[#3D9B80] transition duration-200 transform hover:scale-105"
          >
            Next
          </button>
        </div>
      </main>

      {/* Footer */}
      <footer className="bg-[#4CA1AF] text-white py-4 text-center shadow-md">
        <p>&copy; 2024 Rick and Morty Fan Page</p>
      </footer>
    </div>
  );
};

export default Home;

4. Create the EpisodeCard Component

Create a new directory called components/common and create a file named EpisodeCard.tsx:

mkdir -p components/common
touch components/common/EpisodeCard.tsx


Add the following content to EpisodeCard.tsx to display each episode’s information:

import { EpisodeCardProps } from "@/interfaces";

const EpisodeCard = ({ id, name, air_date, episode }: EpisodeCardProps) => {
  return (
    <div
      key={id}
      className="bg-white cursor-pointer shadow-md rounded-lg p-4 m-4 transition-transform duration-200 hover:scale-105"
    >
      <div className="flex justify-between items-center">
        <h2 className="text-xl font-semibold text-gray-800">{name}</h2>
        <span className="border px-2 text-xs rounded-full bg-blue-600 text-white flex items-center">
          {episode}
        </span>
      </div>
      <p className="text-gray-600">{air_date}</p>
    </div>
  );
};

export default EpisodeCard;

5. Run the Project

Once you have added the necessary files and code, run the following command to start the Next.js server:

npm run dev


Open your browser and navigate to http://localhost:3000 to view the episode list with pagination and the ability to click through pages of episodes.

File Structure

After completing these steps, your file structure should look like this:

alx-rick-and-morty-app/
├── components/
│   └── common/
│       └── EpisodeCard.tsx
├── graphql/
│   └── queries.ts
├── interfaces/
│   └── index.ts
├── pages/
│   └── index.tsx
└── styles/
    └── globals.css

Conclusion

In this task, you have successfully learned how to:

Set up Apollo Client in a React project.

Create a GraphQL query to retrieve Rick and Morty episodes.

Implement pagination to navigate through pages of episodes.

Create reusable components to display episode details.

Now, you can continue to enhance the application further or integrate additional functionality based on the Rick and Morty GraphQL API.
